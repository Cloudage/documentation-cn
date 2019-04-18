#ShaderLab：旧版纹理组合器

在计算基本顶点光照之后将应用纹理。在 ShaderLab 中使用 **SetTexture** 命令来完成此操作。

***注意：**使用[片元程序](SL-ShaderPrograms.html)时，SetTexture 命令无效；因为在这种情况下，像素运算在着色器中完整描述。建议现在使用可编程着色器，而不是 SetTexture 命令。*

可在固定函数纹理中实现旧式组合器效果。您可以在一个通道中包含多个 SetTexture 命令；所有纹理都按顺序应用，就像绘画程序中的图层一样。SetTexture 命令必须放在 [Pass](SL-Pass.html) 的末尾。



##语法


````
SetTexture [TextureName] {Texture Block}
````
分配纹理。_TextureName_ 必须定义为纹理属性。在 _TextureBlock_ 中定义如何应用纹理。

纹理代码块控制着应用纹理的方式。在纹理代码块内部，最多可以有两条命令：`combine` 和 `constantColor`。


##纹理代码块 `combine` 命令



`combine` _src1_ \* _src2_：将 src1 和 src2 相乘。结果将比任一个输入更暗。

`combine` _src1_ + _src2_：将 src1 与 src2 相加。结果将比任一个输入更亮。

`combine` _src1_ - _src2_：从 src1 减去 src2。

`combine` _src1_ `lerp` (_src2_) _src3_：使用 src2 的 Alpha 在 src3 和 src1 之间插值。请注意，插值方向是相反的：当 Alpha 为 1 时使用 src1，而当 Alpha 为 0 时使用 src3。

`combine` _src1_ \* _src2_ + _src3_：将 src1 与 src2 的 Alpha 分量相乘，然后加 src3。

所有 **src** 属性均可为 _previous_、_constant_、_primary_ 或 _texture_ 之一。

* **Previous** 表示上一个 SetTexture 的结果。
* **Primary** 表示[光照计算](SL-Material.html)产生的颜色或者是顶点颜色（如果已[绑定](SL-BindChannels.html)）。
* **Texture** 表示由 SetTexture 中 _TextureName_ 指定的纹理的颜色（请参阅上文）。
* **Constant** 表示 **ConstantColor** 中指定的颜色。

修饰符：

* 上面指定的公式可以选择性地跟随关键字 **Double** 或 **Quad**，作用是让生成的颜色在亮度上乘以 2 或乘以 4。
* 除了 `lerp` 参数之外，所有 **src** 属性都可以选择以 **one -** 开头，作用是对生成的颜色取非。
* 所有 **src** 属性都可以后跟 **alpha**，作用是仅采用 Alpha 通道。


##纹理代码块 `constantColor` 命令

**ConstantColor color：**定义可在 combine 命令中使用的恒定颜色。


##Unity 5.0 中删除的功能

Unity 5.0 之前的版本支持纹理坐标变换（通过在纹理代码块内部使用 `matrix` 命令）。如果现在需要此功能，可考虑将着色器重写为[可编程着色器](SL-ShaderPrograms.html)，并在顶点着色器中进行 UV 变换。

同样，5.0 版本中删除了有符号的加法 (`a+-b`)、乘以有符号的加法 (`a*b+-c`)、先乘后减 (`a*b-c`) 和点积（`dot3` 或 `dot3rgba`）纹理组合模式。如果需要它们，请在像素着色器中进行数学运算。


##详细信息

在[片元程序](SL-ShaderPrograms.html)问世之前，较旧的显卡使用分层的纹理方法。这种方法逐一应用纹理，从而修改将写入屏幕的颜色。对于每个纹理，纹理通常与上一个运算的结果组合。而现在建议使用实际片元程序。


![](../uploads/SL/SetTextureGraph.png) 

请注意，每个纹理阶段可能会也可能不会被限制为 0 至 1 的范围，具体取决于平台。如果 SetTexture 阶段可能生成大于 1.0 的值，这可能会影响这些阶段。


##单独计算 Alpha 和颜色

默认情况下，组合器公式用于计算颜色的 RGB 和 Alpha 分量。您也可以选择指定单独的公式进行 Alpha 计算。如下所示：


````
SetTexture [_MainTex] { combine previous * texture, previous + texture }
````

在这个公式中，我们将 RGB 颜色相乘并与 Alpha 相加。


##镜面高光

默认情况下，**primary** 颜色是漫射颜色、环境颜色和镜面反射颜色的总和（在[光照计算](SL-Material.html)中定义）。如果在通道选项中指定 **SeparateSpecular On**，则镜面反射颜色将在组合器计算_之后_相加，而不是在之前相加。这是内置 VertexLit 着色器的默认行为。


##图形硬件支持

支持[片元着色器](SL-ShaderPrograms.html)的现代显卡（桌面端的“着色器模型 2.0”，移动设备端的 OpenGL ES 2.0）支持所有 __SetTexture__ 模式和至少 4 个纹理阶段（其中许多支持 8 个）。如果使用非常旧的硬件（对于 PC 是指 2003 年之前制造的硬件，对于移动设备是指早于 iPhone3GS 的硬件），最少可能只有两个纹理阶段。着色器作者应为他们想要支持的显卡单独编写[子着色器](SL-SubShader.html)。


##示例



###对两个纹理进行 Alpha 混合


这个简短示例需要两个纹理。首先它将第一个组合器设置为只接受 **\_MainTex**，然后使用 **\_BlendTex** 的 Alpha 通道淡入 **\_BlendTex** 的 RGB 颜色



````
Shader "Examples/2 Alpha Blended Textures" {
    Properties {
        _MainTex ("Base (RGB)", 2D) = "white" {}
        _BlendTex ("Alpha Blended (RGBA) ", 2D) = "white" {}
    }
    SubShader {
        Pass {
            // 应用基础纹理
            SetTexture [_MainTex] {
                combine texture
            }
            // 使用 lerp 运算符混入 Alpha 纹理
            SetTexture [_BlendTex] {
                combine texture lerp (texture) previous
            }
        }
    }
}
````


###Alpha 控制的自发光

此着色器使用 **\_MainTex** 的 Alpha 分量来决定应用光照的位置。它通过将纹理应用于两个阶段来实现这一点；在第一阶段，纹理的 Alpha 值用于在顶点颜色和纯白色之间进行混合。在第二阶段，与纹理的 RGB 值相乘。




````
Shader "Examples/Self-Illumination" {
    Properties {
        _MainTex ("Base (RGB) Self-Illumination (A)", 2D) = "white" {}
    }
    SubShader {
        Pass {
            // 设置基本的白色顶点光照
            Material {
                Diffuse (1,1,1,1)
                Ambient (1,1,1,1)
            }
            Lighting On

            // 使用纹理 Alpha 混合到白色（= 完全光照）
            SetTexture [_MainTex] {
                constantColor (1,1,1,1)
                combine constant lerp(texture) previous
            }
            // 在纹理中相乘
            SetTexture [_MainTex] {
                combine previous * texture
            }
        }
    }
}
````


不过，我们可以在这里任意执行其他某些操作；不必混合为纯白色，我们可以添加自发光颜色并混合到该颜色。注意使用 **ConstantColor** 从属性中获取 _SolidColor 到纹理混合中。




````
Shader "Examples/Self-Illumination 2" {
    Properties {
        _IlluminCol ("Self-Illumination color (RGB)", Color) = (1,1,1,1)
        _MainTex ("Base (RGB) Self-Illumination (A)", 2D) = "white" {}
    }
    SubShader {
        Pass {
            // 设置基本的白色顶点光照
            Material {
                Diffuse (1,1,1,1)
                Ambient (1,1,1,1)
            }
            Lighting On

            // 使用纹理 Alpha 混合到白色（= 完全光照）
            SetTexture [_MainTex] {
                // 将颜色属性拉入此 Blender
                constantColor [_IlluminCol]
                // 并使用纹理的 Alpha 在它和顶点颜色之间
                // 进行混合
                combine constant lerp(texture) previous
            }
            // 在纹理中相乘
            SetTexture [_MainTex] {
                combine previous * texture
            }
        }
    }
}
````


最后，我们获取 VertexLit 着色器的所有光照属性并将其拉入：



````
Shader "Examples/Self-Illumination 3" {
    Properties {
        _IlluminCol ("Self-Illumination color (RGB)", Color) = (1,1,1,1)
        _Color ("Main Color", Color) = (1,1,1,0)
        _SpecColor ("Spec Color", Color) = (1,1,1,1)
        _Emission ("Emmisive Color", Color) = (0,0,0,0)
        _Shininess ("Shininess", Range (0.01, 1)) = 0.7
        _MainTex ("Base (RGB)", 2D) = "white" {}
    }

    SubShader {
        Pass {
            // 设置基本顶点光照
            Material {
                Diffuse [_Color]
                Ambient [_Color]
                Shininess [_Shininess]
                Specular [_SpecColor]
                Emission [_Emission]
            }
            Lighting On

            // 使用纹理 Alpha 混合到白色（= 完全光照）
            SetTexture [_MainTex] {
                constantColor [_IlluminCol]
                combine constant lerp(texture) previous
            }
            // 在纹理中相乘
            SetTexture [_MainTex] {
                combine previous * texture
            }
        }
    }
}
````
