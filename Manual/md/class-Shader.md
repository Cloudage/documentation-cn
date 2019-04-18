# 着色器资源

着色器是包含显卡执行代码和指令的资源。
[材质](class-Material.html)将引用着色器并设置其参数（纹理、颜色等）。

Unity 包含一些在项目中始终可用的内置着色器（例如，
[标准着色器](shader-StandardShader.html)）。您也可以[编写自己的着色器](ShadersOverview.html)并应用[后期处理效果](PostProcessingOverview.html)。


## 创建新着色器

要创建新着色器，请从主菜单或 __Project 视图__上下文菜单中
选择 __Assets__ &gt; __Create__ &gt; __Shader__。着色器是
类似于 C# 脚本的文本文件，以 Cg/HLSL 和 ShaderLab 语言的组合编写而成
（请参阅[编写着色器](ShadersOverview.html)页面以了解详细信息）。

![着色器检视面板 (Shader Inspector)。](../uploads/Shaders/Inspector-Shader.png)


### 着色器导入设置

在此检视面板部分可为着色器指定默认纹理。每当使用
此着色器创建新[材质](class-Material.html)时，都会自动分配
这些纹理。


### 着色器检视面板

着色器检视面板显示有关着色器的基本信息（主要是[着色器标签](SL-SubShaderTags.html)），
并允许编译和检查低级编译代码。

对于[表面着色器 (Surface Shader)](SL-SurfaceShaders.html)，__Show generated code__
按钮将显示 Unity 生成的用于处理光照和阴影的所有代码。如果确实想要
自定义生成的代码，只需将其全部复制并粘贴回原始着色器文件
并开始调整即可。


![着色器编译弹出菜单。](../uploads/Shaders/Inspector-ShaderCompilePopup.png)

__Compile and show code__ 按钮的弹出菜单可检查选定平台
最终编译的着色器代码（例如 Direct3D9 上的程序集，或适用于 OpenGL ES 的
低级优化 GLSL）。此功能在优化着色器性能时非常有用；通常情况下，您会想知道最后在此处生成了
多少低级指令。

生成的低级代码可用于粘贴到 GPU 着色器性能分析工具（如
[AMD GPU ShaderAnalyzer](http://developer.amd.com/tools-and-sdks/graphics-development/gpu-shaderanalyzer/)
或 [PVRShaderEditor](http://community.imgtec.com/developers/powervr/tools/pvrshadereditor/)）。


## 着色器编译详细信息

在着色器导入时，Unity 不会编译整个着色器。这是因为大多数着色器
内部都有很多[变体](SL-MultipleProgramVariants.html)，因此为所有可能的平台编译所有这些变体
将需要很长时间。实际的操作过程为：

* 在导入时，仅对着色器进行最低限度的处理（表面着色器生成等）。
* 只在需要时实际编译着色器变体。
* 不会在导入时执行编译 100-10000 个内部着色器的典型工作，此过程通常最终只编译少数着色器。

在播放器构建时，所有“尚未编译”的着色器变体都将被编译，因此即使 Editor 不会使用这些着色器变体，它们也会存在于游戏数据中。

但是，这确实意味着着色器可能具有在着色器导入时未检测到的
错误。例如，正在使用 Direct3D 11 运行 Editor，但如果着色器是针对 OpenGL 进行编译的，
则会出错。此外，还可能着色器的某些[变体](SL-MultipleProgramVariants.html)不符合着色器模型 2.0
指令限制等。如果 Editor 需要了解这些错误，它们将显示在检视面板中；但是，为了检查错误，手动为所需平台
完全编译着色器也是一种很好的做法。使用着色器检视面板
中的 __Compile and show code__ 弹出菜单即可执行此操作。

着色器编译是使用名为 `UnityShaderCompiler` 的后台进程执行的，该进程由 Unity
在需要编译着色器时即时启动。可启动多个编译器进程（通常在机器中每个 CPU
核心对应一个），这样在播放器构建时就可以并行完成着色器编译。当
Editor 不编译着色器时，编译器进程不执行任何操作，也不消耗计算机资源，
所以不必担心它们。Unity Editor 退出时也将关闭这些进程。

各个着色器变体编译结果将缓存在项目中的 `Library/ShaderCache` 文件夹下。
这意味着 100% 相同的着色器或其代码片段将重用以前编译的结果。此外还
意味着，如果有许多经常更改的着色器，着色器缓存文件夹可能会变得
非常大。删除文件夹内容通常是安全的；只会导致重新编译着色器变体。



## 阅读更多信息

* [材质](class-Material.html)资源。
* [编写着色器概述](ShadersOverview.html)。
* [着色器参考](SL-Reference.html)。

