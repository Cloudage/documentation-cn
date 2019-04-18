编写顶点和片元着色器
===================================


__ShaderLab__ 着色器不止包含“硬件着色器”。它们执行很多操作。它们描述了在材质检视面板中显示的属性，包含用于不同图形硬件的多个着色器实现，配置固定函数硬件状态，等等。实际的可编程着色器（如顶点和片元程序）只是整个 ShaderLab 的“着色器”概念的一部分。有关基本介绍，请查看[着色器教程](ShaderTut2.html)。在本文中，我们将低级别硬件着色器称为__着色器程序__。

**如果要编写与光照交互的着色器，请查看[表面着色器](SL-SurfaceShaders.html)文档**。如需查看一些示例，请参阅[__顶点和片元着色器示例__](SL-VertexFragmentShaderExamples.html)。本页的其余部分假设着色器不与 Unity 光源（例如特殊效果、[后期处理效果](PostProcessingOverview.html)等等）交互。

着色器程序用 [HLSL 语言](SL-ShadingLanguage.html)编写，在着色器文本中嵌入“代码片段”（在 [Pass](SL-Pass.html) 命令中的某个位置）。着色器程序通常如下所示：

````
  Pass {
        // ...常规通道状态设置 ...
      
        CGPROGRAM
        // 此代码片段的编译指令，例如：
        #pragma vertex vert
        #pragma fragment frag
      
        // Cg/HLSL 代码本身
      
        ENDCG
        // ...通道设置的剩余部分 ...
    }
````


## HLSL 代码片段


HLSL 程序代码片段写入到 __CGPROGRAM__ 和 __ENDCG__ 关键字之间，或者 __HLSLPROGRAM__ 和 __ENDHLSL__ 之间。后一种形式*不会*自动包含 HLSLSupport 和 UnityShaderVariables [内置头文件](SL-BuiltinIncludes.html)。

在代码片段的开头，可使用 __#pragma__ 语句的形式提供编译指令。以下指令可指示要编译的着色器函数：

* __#pragma vertex__ _name_ - 作为顶点着色器来编译函数 _name_。
* __#pragma fragment__ _name_ - 作为片元着色器来编译函数 _name_。
* __#pragma geometry__ _name_ - 作为 DX10 几何着色器来编译函数 _name_。如下所述，设置此选项会自动开启 __#pragma target 4.0__。
* __#pragma hull__ _name_ - 作为 DX11 外壳着色器来编译函数 _name_。如下所述，设置此选项会自动开启 __#pragma target 5.0__。
* __#pragma domain__ _name_ - 作为 DX11 域着色器来编译函数 _name_。如下所述，设置此选项会自动开启 __#pragma target 5.0__。

其他编译指令：

* __#pragma target__ _name_ - 要编译到的着色器目标。有关详细信息，请参阅[着色器编译目标](SL-ShaderCompileTargets.html)页面。
* __#pragma require__ _feature_ ...- 对着色器需要的 GPU 功能进行精细控制，请参阅[着色器编译目标](SL-ShaderCompileTargets.html)页面以了解详细信息。
* __#pragma only_renderers__ _空格分隔的名称_ - 仅为给定的渲染器编译着色器。默认情况下，将为所有渲染器编译着色器。请参阅下文中的_渲染器_以了解详细信息。
* __#pragma exclude_renderers__ _空格分隔的名称_ - 不为给定渲染器编译着色器。默认情况下，将为所有渲染器编译着色器。请参阅下文中的_渲染器_以了解详细信息。
* __#pragma multi_compile ...__ - 用于处理[多个着色器变体](SL-MultipleProgramVariants.html)。
* __#pragma enable_d3d11_debug_symbols__ - 为针对 DirectX 11 编译的着色器生成调试信息，这将允许您通过 Visual Studio 2012（或更高版本）图形调试器来调试着色器。
* __#pragma hardware_tier_variants__ _渲染器名称_ - 为每个可以运行所选渲染器的硬件层生成每个编译着色器的[多个着色器硬件变体](SL-MultipleProgramVariants.html)。请参阅下文中的_渲染器_以了解详细信息。

每个代码片段必须至少包含一个顶点程序和一个片元程序。因此，__#pragma vertex__ 和 __#pragma fragment__ 指令是必需的。


从 Unity 5.0 开始不执行任何操作并且可以安全删除的编译指令：`#pragma glsl`、`#pragma glsl_no_auto_normalization`、`#pragma profileoption` 和 `#pragma fragmentoption`。

Unity only supports __#pragma__ directives in the shader files, and not in the includes.

## 渲染平台


Unity 支持多种渲染 API（例如 Direct3D 11 和 OpenGL），默认情况下，所有着色器程序都编译到所有支持的渲染器中。您可以使用 __#pragma only_renderers__ 或 __#pragma exclude_renderers__ 指令指示要编译到的渲染器。如果您确信自己显式使用的一些着色器语言功能在某些平台上无法实现，这样做将非常有用。支持的渲染器名称包括：

* __d3d11__ - Direct3D 11/12
* __glcore__ - OpenGL 3.x/4.x
* __gles__ - OpenGL ES 2.0
* __gles3__ - OpenGL ES 3.x
* __metal__ - iOS/Mac Metal
* __vulkan__ - Vulkan
* __d3d11_9x__ - Direct3D 11 9.x 功能级别，通常在 WSA 平台上使用
* __xboxone__ - Xbox One
* __ps4__ - PlayStation 4
* __psp2__ - PlayStation Vita
* __n3ds__ - Nintendo 3DS


例如，以下行仅会将着色器编译到 D3D11 模式：

	#pragma only_renderers d3d11


## 另请参阅

* [访问材质属性](SL-PropertiesInPrograms.html)。
* [编写多个着色器程序变体](SL-MultipleProgramVariants.html)。
* [着色器编译目标](SL-ShaderCompileTargets.html)。
* [着色语言详细信息](SL-ShadingLanguage.html)。
* [着色器预处理器宏](SL-BuiltinMacros.html)。
* [平台特定的渲染差异](SL-PlatformDifferences.html)。

---

* <span class="page-edit">2018-03-20  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>
* <span class="page-history">在 Unity 2018.1 中添加了着色器 #pragma 指令</span>
