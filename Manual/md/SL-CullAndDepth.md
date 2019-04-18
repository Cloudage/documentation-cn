#ShaderLab：剔除和深度测试

![](../uploads/SL/PipelineCullDepth.png) 

剔除是一种优化方式，即不渲染背离观察者的多边形。所有多边形都有正面和背面。剔除利用大多数对象均为封闭体这一事实；如果您有一个立方体，您将永远不会看到背离您的面（前方始终只有一面朝向您），所以我们不需要绘制背离的面。因此有了“背面剔除”一词。

让渲染看起来正确的另一个功能是“深度测试”。深度测试确保在场景中仅绘制距离最近的表面对象。



##语法

###Cull
````
Cull Back | Front | Off
````
控制应该剔除多边形的哪些面（不绘制）

* **Back** 不渲染背离观察者的多边形_（默认值）_。

* **Front** 不渲染面向观察者的多边形。用于从里到外翻转对象。

* **Off** 禁用剔除 - 绘制所有面。用于特殊效果。


###ZWrite
````
ZWrite On | Off
````
控制是否将此对象的像素写入深度缓冲区（默认值为 _On_）。如果要绘制实体对象，请将其保留为 on。如果要绘制半透明效果，请切换到 `ZWrite Off`。有关更多详细信息，请参阅下文。

###ZTest
````
ZTest Less | Greater | LEqual | GEqual | Equal | NotEqual | Always
````
应如何执行深度测试。默认值为 _LEqual_（将前方或远处的对象作为现有对象绘制；隐藏其后面的对象）。

###Offset
````
Offset Factor, Units
````
允许使用两个参数指定深度偏移：_factor_ 和 _units_。_Factor_ 相对于多边形的 X 或 Y 缩放最大 Z 斜率，而 _units_ 缩放最小可分辨深度缓冲区值。因此可强制将一个多边形绘制在另一个多边形上，尽管它们实际上位于相同位置。例如，`Offset 0, -1` 将多边形拉近摄像机并忽略多边形的斜率，而 `Offset -1, -1` 在观察掠射角时进一步拉近多边形。


##示例

此对象将仅渲染对象的背面：

````
Shader "Show Insides" {
    SubShader {
        Pass {
            Material {
                Diffuse (1,1,1,1)
            }
            Lighting On
            Cull Front
        }
    }
}
````

尝试将它应用到一个立方体，并注意当您围绕它转动时几何体感觉是错误的。这是因为您只看到了立方体的内部部分。


###具有深度写入的透明着色器

通常，[半透明着色器](shader-TransparentFamily.html)不会写入到深度缓冲区。但是，这会产生绘制顺序问题，尤其是复杂的非凸网格。如果您想淡入和淡出这样的网格，那么在渲染透明度之前使用填充深度缓冲区的着色器可能会很有用。

![半透明对象；左：标准透明/漫射着色器；右：写入到深度缓冲区的着色器。](../uploads/Main/TransparentDiffuseZWrite.png)

````
Shader "Transparent/Diffuse ZWrite" {
Properties {
    _Color ("Main Color", Color) = (1,1,1,1)
    _MainTex ("Base (RGB) Trans (A)", 2D) = "white" {}
}
SubShader {
    Tags {"Queue"="Transparent" "IgnoreProjector"="True" "RenderType"="Transparent"}
    LOD 200

    // 仅渲染到深度缓冲区的额外通道
    Pass {
        ZWrite On
        ColorMask 0
    }

    // 粘贴在透明/漫射着色器的前向渲染通道中
    UsePass "Transparent/Diffuse/FORWARD"
}
Fallback "Transparent/VertexLit"
}
````


###调试法线
下一种情况更有趣；首先我们用法线顶点光照渲染对象，然后我们用亮粉色渲染背面。这样将产生突出显示需要翻转法线的位置的效果。如果您看到物理控制的对象被任何网格“吸入”，请尝试为它们分配此着色器。如果可以看到任何粉红色的部分，这些部分会吸入任何不幸触及它的东西。

我们开始吧：

````
Shader "Reveal Backfaces" {
    Properties {
        _MainTex ("Base (RGB)", 2D) = "white" { }
    }
    SubShader {
        // 渲染对象的正面部分。
        // 我们使用简单的白色材质，并应用主纹理。
        Pass {
            Material {
                Diffuse (1,1,1,1)
            }
            Lighting On
            SetTexture [_MainTex] {
                Combine Primary * Texture
            }
        }

        // 现在，我们将背面三角形渲染成
        // 世界上最刺激的颜色：亮粉色！
        Pass {
            Color (1,0,1,1)
            Cull Front
        }
    }
}
````


###玻璃剔除
控制剔除的用处不仅限于调试背面。如果您有透明对象，则通常需要显示对象的背面。如果渲染时没有任何剔除 (**Cull Off**)，一些背面可能与一些正面重叠。

下面是一个简单的着色器，适用于凸面体对象（球体、立方体或汽车挡风玻璃）。



````
Shader "Simple Glass" {
    Properties {
        _Color ("Main Color", Color) = (1,1,1,0)
        _SpecColor ("Spec Color", Color) = (1,1,1,1)
        _Emission ("Emmisive Color", Color) = (0,0,0,0)
        _Shininess ("Shininess", Range (0.01, 1)) = 0.7
        _MainTex ("Base (RGB)", 2D) = "white" { }
    }

    SubShader {
        // 我们在子着色器中定义通道以便在多个通道中使用该材质。
        // 此处定义的任何内容都将成为所有包含的通道的默认值。
        Material {
            Diffuse [_Color]
            Ambient [_Color]
            Shininess [_Shininess]
            Specular [_SpecColor]
            Emission [_Emission]
        }
        Lighting On
        SeparateSpecular On

        // 设置 Alpha 混合
        Blend SrcAlpha OneMinusSrcAlpha

        // 渲染对象的背面部分。
        // 如果对象为凸面体，那么这些部分始终
        // 比正面更远。
        Pass {
            Cull Front
            SetTexture [_MainTex] {
                Combine Primary * Texture
            }
        }
        // 渲染面向我们的对象部分。
        // 如果对象为凸面体，那么这些部分将
        // 比背面更近。
        Pass {
            Cull Back
            SetTexture [_MainTex] {
                Combine Primary * Texture
            }
        }
    }
}
````
