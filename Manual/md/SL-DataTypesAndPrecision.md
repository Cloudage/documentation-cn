# 着色器数据类型和精度

Unity 中的标准着色器语言为 [HLSL](SL-ShadingLanguage.html)，支持一般 HLSL 数据类型。但是，Unity 对 HLSL 类型有一些补充，特别是为了在移动平台上提供更好的支持。


## 基本数据类型

着色器中的大多数计算是对浮点数（在 C# 等常规编程语言中为 `float`）进行的。浮点类型有几种变体：`float`、`half` 和 `fixed`（以及它们的矢量/矩阵变体，比如 `half3` 和 `float4x4`）。这些类型的精度不同（因此性能或功耗也不同）：

#### 高精度：`float`

最高精度浮点值；一般是 32 位（就像常规编程语言中的 `float`）。

完整的 `float` 精度通常用于世界空间位置、纹理坐标或涉及复杂函数（如三角函数或幂/取幂）的标量计算。

#### 中等精度：`half`

中等精度浮点值；通常为 16 位（范围为 -60000 至 +60000，精度约为 3 位小数）。

半精度对于短矢量、方向、对象空间位置、高动态范围颜色非常有用。

#### 低精度：`fixed`

最低精度的定点值。通常是 11 位，范围从 -2.0 到 +2.0，精度为 1/256。

固定精度对于常规颜色（通常存储在常规纹理中）以及对它们执行简单运算非常有用。

#### 整数数据类型

整数（`int` 数据类型）通常用作循环计数器或数组索引。为此，它们通常可以在各种平台上正常工作。

根据平台的不同，GPU 可能不支持整数类型。例如，Direct3D 9 和 OpenGL ES 2.0 GPU 仅对浮点数据进行运算，并且可以使用相当复杂的浮点数学指令来模拟简单的整数表达式（涉及位运算或逻辑运算）。

Direct3D 11、OpenGL ES 3、Metal 和其他现代平台都对整数数据类型有适当的支持，因此使用位移位和位屏蔽可以按预期工作。


## 复合矢量/矩阵类型

HLSL 具有从基本类型创建的内置矢量和矩阵类型。例如，`float3` 是一个 3D 矢量，具有分量 .x、.y 和 .z，而 `half4` 是一个中等精度 4D 矢量，具有分量 .x、.y、.z 和 .w。或者，可使用 .r、.g、.b 和 .a 分量来对矢量编制索引，这在处理颜色时很有用。

矩阵类型以类似的方式构建；例如 `float4x4` 是一个 4x4 变换矩阵。请注意，某些平台仅支持方形矩阵，最主要的是 OpenGL ES 2.0。


## 纹理/采样器类型

通常按照如下方式在 HLSL 代码中声明纹理：

```
sampler2D _MainTex;
samplerCUBE _Cubemap;
```

对于移动平台，这些将转换为“低精度采样器”，即预期纹理应具有低精度数据。如果您知道纹理包含 HDR 颜色，则可能需要使用半精度采样器：

```
sampler2D_half _MainTex;
samplerCUBE_half _Cubemap;
```

或者，如果纹理包含完整浮点精度数据（例如[深度纹理](SL-DepthTextures.html)），请使用完整精度采样器：

```
sampler2D_float _MainTex;
samplerCUBE_float _Cubemap;
```



## 精度、硬件支持和性能

使用 `float`/`half`/`fixed` 数据类型的一个难题是：PC GPU **始终**为高精度。也就是说，对于所有 PC (Windows/Mac/Linux) GPU，在着色器中编写 `float`、`half` 还是 `fixed` 数据类型都无关紧要。这些 GPU 将始终以 32 位浮点精度来计算所有数据。

仅当目标平台是移动端 GPU 时，`half` 和 `fixed` 类型才变得重要，在这种情况下，这些类型主要面临功耗（有时候是性能）约束。请记住，要确认是否遇到精度/数值问题，必须在移动设备上测试着色器。

即使在移动端 GPU 上，不同的精度支持也会因 GPU 产品系列而异。下面概述了个每个移动端 GPU 产品系列如何处理每个浮点类型（以用于该产品系列的位数来表示）：


| GPU 产品系列 | 浮点精度 | 半精度 | 固定精度    |
|:---|:---|:---|:---|
|PowerVR 系列 6/7     | 32 | 16     ||
|PowerVR SGX 5xx        | 32 | 16 | 11 |
|Qualcomm Adreno 4xx/3xx| 32 | 16     ||
|Qualcomm Adreno 2xx    | 32 顶点，24 片元 |||
|ARM Mali T6xx/7xx      | 32 | 16     ||
|ARM Mali 400/450       | 32 顶点，16 片元       |||
|NVIDIA X1              | 32 | 16     ||
|NVIDIA K1              | 32         |||
|NVIDIA Tegra 3/4       | 32 |16      ||

大多数现代移动端 GPU 实际上只支持 32 位数字（用于 `float` 类型）或 16 位数字（用于 `half` 和 `fixed` 类型）。一些较旧的 GPU 对顶点着色器和片元着色器计算具有不同的精度。

使用较低的精度通常可以更快，这可能是由于改进的 GPU 寄存器分配，或是由于某些低精度数学运算的特殊“快速路径”执行单元。即使没有原始性能优势，使用较低的精度通常也会降低 GPU 的功耗，从而延长电池续航时间。

一般的经验法则是全部都从半精度开始（但位置和纹理坐标除外）。仅当半精度对于计算的某些部分不足时，才增加精度。


#### 支持无穷大、非数字和其他特殊浮点值

对特殊浮点值的支持可能会有所不同，具体取决于运行的 GPU 产品系列（主要是移动端）。

支持 Direct3D 10 的所有 PC GPU 都支持非常明确的 IEEE 754 浮点标准。这意味着，在 CPU 上，浮点数的行为与常规编程语言完全相同。

移动端 GPU 的支持程度可能稍有不同。在某些移动端 GPU 中，将零除以零可能会导致 NaN（“非数字”）；在其他移动端 GPU 上，它可能会导致无穷大、零或任何其他不明值。务必在目标设备上测试着色器以检查着色器是否受支持。


## 外部 GPU 文档

GPU 供应商会提供有关其 GPU 性能和功能的深入指南。请参阅下文以了解详情：

* [适用于 Unity 开发者的 ARM Mali 指南 (ARM Mali Guide for Unity Developers)](https://developer.arm.com/products/software-development-tools/graphics-development-tools/mali-graphics-debugger/docs/100140/0303)
* [Qualcomm Adreno OpenGL ES 开发者指南 (Qualcomm Adreno OpenGL ES Developer Guide)](https://developer.qualcomm.com/software/adreno-gpu-sdk/tools)
* [PowerVR 架构指南 (PowerVR Architecture Guides)](https://community.imgtec.com/developers/powervr/documentation/)


## 另请参阅

* [平台特定的渲染差异](SL-PlatformDifferences.html)
* [着色器性能提示](SL-ShaderPerformance.html)
* [着色语言](SL-ShadingLanguage.html)
