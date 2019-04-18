# Unity 中使用的着色语言

在 Unity 中，[着色器程序](SL-ShaderPrograms.html)是用 [HLSL](https://en.wikipedia.org/wiki/High-Level_Shading_Language) 语言的一个变种（也称为 [Cg](https://en.wikipedia.org/wiki/Cg_%28programming_language%29)，但对于大部分实际使用情况，这两种语言没有区别）编写的。

目前，为了在不同平台之间实现最大程度的可移植性，请使用 DX9 风格的 HLSL 编写程序（例如，使用 DX9 风格的 `sampler2D` 和 `tex2D` 进行纹理采样，而不是使用 DX10 风格的 `Texture2D`、`SamplerState` 和 `tex.Sample`）。

## 着色器编译器

在内部将使用不同的着色器编译器来编译[着色器程序](SL-ShaderPrograms.html)：

* Windows 和 Microsoft 平台（DX11、DX12 和 Xbox One）全部使用 Microsoft 的 HLSL 编译器（最新版本为 d3dcompiler_47）。
* OpenGL Core、OpenGL ES 3 和 Metal 使用 Microsoft 的 HLSL，然后使用 [HLSLcc](https://github.com/Unity-Technologies/HLSLcc) 按字节代码转换为 GLSL 或 Metal。
* OpenGL ES 2.0 通过 [hlsl2glslfork](https://github.com/aras-p/hlsl2glslfork) 和 [glsl 优化器](https://github.com/aras-p/glsl-optimizer)进行源代码级别转换。
* 其他游戏主机平台使用其各自的编译器（例如，PS4 使用 PSSL）。
* [表面着色器](SL-SurfaceShaders.html)使用 Cg 2.2 和 [MojoShader](https://icculus.org/mojoshader/) 来完成代码生成分析步骤。


如果确实需要确定正在使用哪个编译器（为了使用仅一种编译器支持的 HLSL 语法，或解决编译器错误），可以使用[预定义的着色器宏](SL-BuiltinMacros.html)。例如，使用 HLSL 编译器进行编译时设置 `UNITY_COMPILER_HLSL`（针对 D3D 或 GLCore/GLES3 平台）；通过 hlsl2glsl 进行编译时设置 `UNITY_COMPILER_HLSL2GLSL`。


## 另请参阅

* [着色器程序](SL-ShaderPrograms.html)。
* [着色器预处理器宏](SL-BuiltinMacros.html)。
* [平台特定的渲染差异](SL-PlatformDifferences.html)。
