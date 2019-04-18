# 计算着色器

计算着色器是在显卡上并位于正常渲染管线之外运行的程序。它们可用于大规模并行的 GPGPU 算法，或用于加速游戏渲染的某些部分。为了有效使用它们，通常需要深入了解 GPU 架构和并行算法；并了解 [DirectCompute](http://msdn.microsoft.com/en-us/library/windows/desktop/ff476331.aspx)、[OpenGL Compute](https://www.khronos.org/opengl/wiki/Compute_Shader)、[CUDA](http://en.wikipedia.org/wiki/CUDA) 或 [OpenCL](http://en.wikipedia.org/wiki/OpenCL)。

Unity 中的计算着色器与 [DirectX](https://en.wikipedia.org/wiki/DirectX) 11 DirectCompute 技术紧密配合。计算着色器适用的平台：

* Windows 和 Windows 应用商店，使用 DirectX 11 或 DirectX 12 图形 API 和 Shader Model 5.0 GPU

* macOS 和 iOS，使用 [Metal 图形](https://developer.apple.com/metal/) API

* Android、Linux 和 Windows 平台，[Vulkan](https://www.khronos.org/vulkan/) API

* 现代 [OpenGL](https://www.opengl.org/) 平台（Linux 或 Windows 上的 OpenGL 4.3；Android 上的 OpenGL ES 3.1）。请注意，Mac OS X 不支持 OpenGL 4.3

* 现代游戏主机（Sony PS4 和 Microsoft Xbox One）

在运行时可使用 [SystemInfo.supportsComputeShaders](../ScriptReference/SystemInfo-supportsComputeShaders.html) 来查询计算着色器支持情况。

## 计算着色器资源

类似于[常规着色器](class-Shader.html)，计算着色器是项目中的资源文件，文件扩展名为 *.compute*。它们是以 DirectX 11 样式 [HLSL](http://msdn.microsoft.com/en-us/library/windows/desktop/bb509561.aspx) 语言编写的，具有最少数量的 #pragma 编译指令来指示哪些函数将编译为计算着色器内核。

下面是计算着色器文件的基本示例，它使用红色填充输出纹理：

```
// test.compute

# pragma kernel FillWithRed

RWTexture2D<float4> res;

[numthreads(1,1,1)]
void FillWithRed (uint3 dtid : SV_DispatchThreadID)
{
    res[dtid.xy] = float4(1,0,0,1);
}
```

此处的语言是标准 [DX11 HLSL](SL-ShadingLanguage.html)，具有附加的 `#pragma kernel FillWithRed` 指令。一个计算着色器资源文件必须包含至少一个可以调用的 `compute kernel`，该函数由 `#pragma directive` 指示。文件中可以有更多内核；只需添加多个 `#pragma kernel` 行。

使用多个 `#pragma kernel` 行时，请注意在 `#pragma kernel` 指令的同一行上不允许 `// text` 样式的注释，如果使用，会导致编译错误。

可选择性地在 `#pragma kernel` 行后面添加要在编译该内核时定义的多个预处理器宏，例如：

```
# pragma kernel KernelOne SOME_DEFINE DEFINE_WITH_VALUE=1337
# pragma kernel KernelTwo OTHER_DEFINE
// ...
```

## 调用计算着色器

在脚本中，应定义 ComputeShader 类型的变量并分配对资源的引用。如此便可使用 [ComputeShader.Dispatch](../ScriptReference/ComputeShader.Dispatch.html) 函数来调用它们。请参阅关于 [ComputeShader 类](../ScriptReference/ComputeShader.html)的 Unity 文档以了解更多详细信息。

与计算着色器密切相关的是 [ComputeBuffer](../ScriptReference/ComputeBuffer.html) 类，该类将定义任意数据缓冲区（在 DX11 术语中称为“结构化缓冲区”）。如果已设置“随机访问”标志（在 DX11 中称为“无序访问视图”），也可从计算着色器中写入[渲染纹理](../ScriptReference/RenderTexture.html)。请参阅 [RenderTexture.enableRandomWrite](../ScriptReference/RenderTexture-enableRandomWrite.html) 以了解与此相关的更多信息。

## 计算着色器中的纹理采样器

纹理和采样器不是 Unity 中的单独对象，因此要在计算着色器中使用它们，必须遵循以下 Unity 特定规则之一：

* 使用与纹理名称相同的名称，以 `sampler` 开头（例如，`Texture2D MyTex`；`SamplerState samplerMyTex`）。在此情况下，采样器将初始化为纹理的过滤/包裹/各向异性 (filter/wrap/aniso) 设置。

* 使用预定义采样器。因此，该名称必须具有 `Linear` 或 `Point`（对于过滤模式）和 `Clamp` 或 `Repeat`（包裹模式）。例如，`SamplerState MyLinearClampSampler` 会创建一个具有线性过滤模式和钳制包裹模式的采样器。

有关更多信息，请参阅[采样器状态](SL-SamplerStates.html)文档。

## 跨平台支持

与常规着色器一样，Unity 可将计算着色器从 HLSL [转换](SL-ShadingLanguage.html)为其他着色器语言。因此，对于最简单的跨平台版本，应以 HLSL 编写计算着色器。但是，在执行此操作时需要考虑一些因素。

### 跨平台最佳实践

DirectX 11 (DX11) 支持在其他平台（如 [Metal](https://developer.apple.com/metal/) 或 [OpenGL ES](https://www.opengl.org/)）上不支持的许多操作。因此，应始终确保着色器在提供更少支持的平台（而不是仅在 DX11 上）上具有良好定义的行为。以下是要考虑的一些事项：

* 越界内存访问是错误的。DX11 在读取时可能始终返回零，并且在读取某些写入时没有问题，但提供较少支持的平台可能会在执行此操作时导致 GPU 崩溃。密切注意特定于 DX11 的破解问题，与线程组大小倍数不匹配的缓冲区大小，试图从缓冲区的开头或结尾读取相邻的数据元素，以及类似的不兼容性。

* 初始化您的资源。新缓冲区和纹理的内容是未定义的。有些平台可能会提供全零，但在其他平台上，可能会有某种内容（包括非数字）。

* 绑定计算着色器声明的所有资源。即使您确定着色器在当前状态下由于分支而没有使用资源，仍必须确保有资源与其绑定。

### 平台特定差异

* [Metal](https://developer.apple.com/metal/)（适用于 iOS 和 tvOS 平台）不支持对纹理的原子操作。Metal 也不支持对缓冲区的 `GetDimensions` 查询。如果需要，请将缓冲区大小信息作为常量传递给着色器。

* [OpenGL ES](https://www.opengl.org/) 3.1（适用于 Android、iOS、tvOS 平台）仅保证一次支持 4 个计算缓冲区。实际的实现通常支持更多数量，但在一般情况下，如果为 OpenGL ES 进行开发，应考虑在结构中对相关数据分组，而不是将每个数据项放在自己的缓冲区中。

## 仅限 HLSL 或仅限 GLSL 的计算着色器

通常情况下会以 [HLSL](https://en.wikipedia.org/wiki/High-Level_Shading_Language) 编写计算着色器文件，并自动将这些文件编译或转换到所有需要的平台中。但是，可以阻止转换为其他语言（即仅保留 HLSL 平台）或者手动编写 [GLSL](https://en.wikipedia.org/wiki/OpenGL_Shading_Language) 计算代码。

以下信息仅适用于仅限 HLSL 或仅限 GLSL 的计算着色器，而不适用于跨平台版本。这是因为此信息可能导致计算着色器源代码被排除在某些平台之外。

* 对于非 HLSL 平台，不会处理 `CGPROGRAM` 和 `ENDCG` 关键字包围的计算着色器源代码。

* `GLSLPROGRAM` 和 `ENDGLSL` 关键字包围的计算着色器源代码视为 GLSL 源代码，并逐字发出。这仅在目标平台为 OpenGL 或 GLSL 平台时才奏效。还应注意，虽然自动转换的着色器遵循缓冲区上的 HLSL 数据布局，但是手动编写的 GLSL 着色器将遵循 GLSL 布局规则。


<br/> 

---

* <span class="page-edit"> 2017-05-18  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 5.6 版中添加：SystemInfo.supportsComputeShaders、平台 macOS、iOS（使用 Metal）、Android、Linux、Windows（具有 Vulkan）
</span>




