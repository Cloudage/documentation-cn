#ShaderLab：旧版光照

材质和光照参数用于控制内置顶点光照。顶点光照是为每个顶点计算的标准 Direct3D/OpenGL 光照模型。__Lighting on__ 可开启光照。光照将受到 __Material__ 代码块、__ColorMaterial__ 命令和 __SeparateSpecular__ 命令的影响。

***注意：**使用[顶点程序](SL-ShaderPrograms.html)时，材质/光照命令无效；因为在这种情况下，所有计算都在着色器中完整描述。建议现在使用可编程着色器，而不是旧版顶点光照。这些情况下，不要使用此处描述的命令，而是自行定义[顶点和片元程序](SL-ShaderPrograms.html)来自己执行所有的光照、纹理和任何其他操作。*


对于任何渲染的几何体来说，第一个要计算的效果就是“顶点着色和光照”。它在顶点级别上运行，并计算在应用纹理之前使用的基色。



##语法

顶级命令控制是否使用固定函数光照，以及一些配置选项。主要设置位于 **Material 代码块**中，下面进行了详细介绍。

###Color
````
	Color color
````
将对象设置为纯色。颜色是用圆括号括起的四个 RGBA 值，或者是用方括号括起的颜色属性名称。

###Material
````
	Material {Material 代码块}
````
Material 代码块用于定义对象的材质属性。

###Lighting
````
	Lighting On | Off
````
要使 Material 代码块中定义的设置生效，必须使用 _Lighting On_ 命令启用光照。如果关闭光照，则直接从 _Color_ 命令获取颜色。

###SeparateSpecular
````
	SeparateSpecular On | Off
````
此命令将镜面反射光照添加到着色器通道的末尾，因此镜面反射光照不受纹理影响。仅当使用 _Lighting On_ 时才有效。

###ColorMaterial
````
	ColorMaterial AmbientAndDiffuse | Emission
````
使用每顶点颜色代替材质中设置的颜色。__AmbientAndDiffuse__ 将替换材质的 Ambient 值和 Diffuse 值；__Emission__ 将替换材质的 Emission 值。


###Material 代码块

此代码块包含有关材质如何对光线做出反应的设置。可省略这些属性中的任何一个，在这种情况下它们默认为黑色（即没有效果）。

**漫射颜色 (Diffuse color)：**漫射颜色分量。这是对象的基色。

**环境颜色 (Ambient color)：**环境颜色分量。这是 [Lighting 窗口](GlobalIllumination.html)中设置的环境光产生的对象光照颜色。

**镜面反射颜色 (Specular color)：**对象的镜面高光颜色。

**光泽度数字 (Shininess number)：**高光的清晰度，介于 0 到 1 之间。如果为 0，您会看到一个巨大的高光，看起来很像漫射光照；如果为 1，您会看到一个微小的斑点。

**发光颜色 (Emission color)：**任何光源均未照射到对象时的对象颜色。

照射到对象的光源的完整颜色为：

  **环境光** \* [Lighting 窗口中的 Ambient Intensity 设置](GlobalIllumination.html) +
   (光源颜色 \* **漫射** + 光源颜色 \* **镜面反射**) + **发光**

对于照射对象的所有光源，重复该等式的光源部分（圆括号内）。

通常情况系会希望保持漫射和环境光颜色相同（Unity 的所有内置着色器都会如此）。


##示例

始终以纯红色渲染对象：


````
Shader "Solid Red" {
    SubShader {
        Pass { Color (1,0,0,0) }
    }
}
````


将对象着色为白色并应用顶点光照的基本着色器：


````
Shader "VertexLit White" {
    SubShader {
        Pass {
            Material {
                Diffuse (1,1,1,1)
                Ambient (1,1,1,1)
            }
            Lighting On
        }
    }
}
````


以下是一种扩展版本，可将材质颜色添加为材质检视面板 (Material Inspector) 中的可见属性：



````
Shader "VertexLit Simple" {
    Properties {
        _Color ("Main Color", COLOR) = (1,1,1,1)
    }
    SubShader {
        Pass {
            Material {
                Diffuse [_Color]
                Ambient [_Color]
            }
            Lighting On
        }
    }
}
````


最后是一个完整的顶点光照着色器（另请参阅 [SetTexture](SL-SetTexture.html) 参考页面）：


````
Shader "VertexLit" {
    Properties {
        _Color ("Main Color", Color) = (1,1,1,0)
        _SpecColor ("Spec Color", Color) = (1,1,1,1)
        _Emission ("Emmisive Color", Color) = (0,0,0,0)
        _Shininess ("Shininess", Range (0.01, 1)) = 0.7
        _MainTex ("Base (RGB)", 2D) = "white" {}
    }
    SubShader {
        Pass {
            Material {
                Diffuse [_Color]
                Ambient [_Color]
                Shininess [_Shininess]
                Specular [_SpecColor]
                Emission [_Emission]
            }
            Lighting On
            SeparateSpecular On
            SetTexture [_MainTex] {
                Combine texture * primary DOUBLE, texture * primary
            }
        }
    }
}
````
