#ShaderLab：混合

混合用于生成透明对象。


![](../uploads/SL/PipelineBlend.png) 

渲染图形时，在执行所有着色器并应用所有纹理后，像素将写入到屏幕。这些像素与已有像素的组合方式由 Blend 命令控制。


##语法

`Blend Off`：关闭混合（这是默认值）

`Blend SrcFactor DstFactor`：配置并启用混合。生成的颜色将乘以 **SrcFactor**。屏幕上的已有颜色乘以 **DstFactor**，然后将这两个值相加。

`Blend SrcFactor DstFactor, SrcFactorA DstFactorA`：同上，但使用不同系数来混合 Alpha 通道。

`BlendOp Op`：不将混合颜色相加，而是对它们执行不同的操作。

`BlendOp OpColor, OpAlpha`：同上，但是对颜色 (RGB) 通道和 Alpha (A) 通道使用不同的混合操作。

此外，您还可以设置上层渲染目标混合模式。当
使用多渲染目标 (MRT) 渲染时，上面的常规语法
将为所有渲染目标设置相同的混合模式。以下语法可以为各个渲染目标设置不同的混合模式，其中 `N` 是渲染目标索引（0 到 7）。此功能适用于大多数现代 API/GPU（DX11/12、GLCore、Metal 和 PS4）：

* `Blend N SrcFactor DstFactor`
* `Blend N SrcFactor DstFactor, SrcFactorA DstFactorA`
* `BlendOp N Op`
* `BlendOp N OpColor, OpAlpha`

`AlphaToMask On`：开启 alpha-to-coverage。使用 MSAA 时，alpha-to-coverage 会根据像素着色器结果 Alpha 值按比例修改多重采样覆盖率遮罩。这通常用于比常规 Alpha 测试更少锯齿的轮廓；对植被和其他经过 Alpha 测试的着色器非常有用。


##混合运算

可使用以下混合运算：


| | |
|:---|:---|
|**Add** |将源和目标相加。 |
|**Sub** |从源减去目标。 |
|**RevSub** |从目标减去源。 |
|**Min** |使用源和目标中的较小者。 |
|**Max** |使用源和目标中的较大者。 |
|**LogicalClear** |逻辑运算：清除 (0) **仅限 DX11.1**。 |
|**LogicalSet** |逻辑运算：设置 (1) **仅限 DX11.1**。 |
|**LogicalCopy** |逻辑运算：复制 (s) **仅限 DX11.1**。 |
|**LogicalCopyInverted** |逻辑运算：逆复制 (!s) **仅限 DX11.1**。 |
|**LogicalNoop** |逻辑运算：空操作 (d) **仅限 DX11.1**。 |
|**LogicalInvert** |逻辑运算：逆运算 (!d) **仅限 DX11.1**。 |
|**LogicalAnd** |逻辑运算：与 (s & d) **仅限 DX11.1**。 |
|**LogicalNand** |逻辑运算：与非 !(s & d) **仅限 DX11.1**。 |
|**LogicalOr** |Logical operation: Or (s \| d) **DX11.1 only**. |
|**LogicalNor** |Logical operation: Nor !(s \| d) **DX11.1 only**. |
|**LogicalXor** |逻辑运算：异或 (s ^ d) **仅限 DX11.1**。 |
|**LogicalEquiv** |逻辑运算：相等 !(s ^ d) **仅限 DX11.1**。 |
|**LogicalAndReverse** |逻辑运算：反转与 (s & !d) **仅限 DX11.1**。 |
|**LogicalAndInverted** |逻辑运算：逆与 (s & d) **仅限 DX11.1**。 |
|**LogicalOrReverse** |Logical operation: Reverse Or (s \| !d) **DX11.1 only**. |
|**LogicalOrInverted** |Logical operation: Inverted Or (!s \| d) **DX11.1 only**. |



##混合系数

以下所有属性对 **Blend** 命令中的 SrcFactor 和 DstFactor 都有效。**源**是指计算所得颜色，**目标**是指屏幕上已有的颜色。如果 **BlendOp** 在使用逻辑运算，则将忽略混合系数。


| | |
|:---|:---|
|**One** |值为 1 - 让源或目标颜色通过。 |
|**Zero** |值为 0 - 删除源或目标值。 |
|**SrcColor** |此阶段的值乘以源颜色值。 |
|**SrcAlpha** |此阶段的值乘以源 Alpha 值。 |
|**DstColor** |此阶段的值乘以帧缓冲区源颜色值。 |
|**DstAlpha** |此阶段的值乘以帧缓冲区源 Alpha 值。 |
|**OneMinusSrcColor** |此阶段的值乘以（1 - 源颜色）。 |
|**OneMinusSrcAlpha** |此阶段的值乘以（1 - 源 Alpha）。 |
|**OneMinusDstColor** |此阶段的值乘以（1 - 目标颜色）。 |
|**OneMinusDstAlpha** |此阶段的值乘以（1 - 目标 Alpha）。 |


##详细信息

以下是最常见的混合类型：

````
Blend SrcAlpha OneMinusSrcAlpha // 传统透明度
Blend One OneMinusSrcAlpha // 预乘透明度
Blend One One // 加法
Blend OneMinusDstColor One // 软加法
Blend DstColor Zero // 乘法
Blend DstColor SrcColor // 2x 乘法
````


## Alpha 混合、Alpha 测试和 alpha-to-coverage

为了绘制大多数完全不透明或完全透明的对象（其中透明度由纹理的 Alpha 通道定义，例如树叶、草、铁丝网等），通常使用几种方法：


#### Alpha 混合

![常规 Alpha 混合](../uploads/SL/AlphaToMask-Blending.png)

这通常意味着对象必须被视为“半透明”，因而无法使用某些渲染功能（例如：延迟着色，无法接受阴影）。凹陷或重叠的 Alpha 混合对象通常也有绘制排序问题。

通常，Alpha 混合着色器还设置透明[渲染队列](SL-SubShaderTags.html)，并关闭深度写入。因此，着色器代码如下所示：

```
// SubShader 内部
Tags { "Queue"="Transparent" "RenderType"="Transparent" "IgnoreProjector"="True" }

// Pass 内部
ZWrite Off
Blend SrcAlpha OneMinusSrcAlpha
```


#### Alpha 测试/镂空

![像素着色器中的 clip()](../uploads/SL/AlphaToMask-clip.png)

通过在像素着色器中使用 `clip()` HLSL 指令，可以根据某些条件对像素进行“废弃”处理。这意味着对象仍然可以被视为完全不透明，并且没有绘制顺序问题。但是，这意味着所有像素都是完全不透明或透明的，导致锯齿（失真）。

通常，经过 alpha 测试的着色器还会设置镂空[渲染队列](SL-SubShaderTags.html)，因此着色器代码如下所示：

```
// SubShader 内部
Tags { "Queue"="AlphaTest" "RenderType"="TransparentCutout" "IgnoreProjector"="True" }

// 片元着色器中的 CGPROGRAM 内部
clip(textureColor.a - alphaCutoffValue);
```



#### Alpha-to-coverage

![AlphaToMask 开启，采用 4xMSAA](../uploads/SL/AlphaToMask-4x.png)

当使用多重采样抗锯齿（MSAA，请参阅 [QualitySettings](class-QualitySettings.html)）时，可通过使用 alpha-to-coverage GPU 功能来改进 Alpha 测试方法。根据使用的 MSAA 级别，这将改善边缘外观。

此功能最适合用于大多数不透明或透明的纹理，并且具有非常薄的“部分透明”区域（草、树叶和类似物）。

通常，alpha-to-coverage 着色器还会设置镂空[渲染队列](SL-SubShaderTags.html)。因此，着色器代码如下所示：

```
// SubShader 内部
Tags { "Queue"="AlphaTest" "RenderType"="TransparentCutout" "IgnoreProjector"="True" }

// Pass 内部
AlphaToMask On
```



##示例

以下是一个小型着色器示例，用于为屏幕上已有的任何内容添加纹理：

````
Shader "Simple Additive" {
    Properties {
        _MainTex ("Texture to blend", 2D) = "black" {}
    }
    SubShader {
        Tags { "Queue" = "Transparent" }
        Pass {
            Blend One One
            SetTexture [_MainTex] { combine texture }
        }
    }
}
````
