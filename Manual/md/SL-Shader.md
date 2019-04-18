#ShaderLab 语法

Unity 中的所有[着色器](class-Shader.html)文件都是用名为“ShaderLab”的声明性语言编写的。在文件中，通过嵌套括号
语法声明着色器的各个描述，例如，材质检视面板中应显示的着色器属性；
要执行哪些类型的硬件回退；要使用哪些类型的混合模式等；而且实际“着色器代码”
编写在同一着色器文件中的 `CGPROGRAM` 代码片段中（请参阅[表面着色器](SL-SurfaceShaders.html)和[顶点和片元着色器](SL-ShaderPrograms.html)）。

本页以及子页面将介绍嵌套括号“ShaderLab”语法。`CGPROGRAM` 代码片段是用常规
HLSL/Cg 着色语言编写的，请参阅[相关文档页面](SL-ShaderPrograms.html)。


__Shader__ 是着色器文件的根命令。每个文件必须定义一个（且仅一个）Shader。Shader 指定当对象的材质使用此着色器时如何渲染此类对象。


##语法

````
Shader "name" { [Properties] Subshaders [Fallback] [CustomEditor] }
````

定义着色器。此着色器将以指定的**名称**显示在材质检视面板中。着色器可以选择性地定义在材质检视面板中显示的**属性**列表。在此之后是子着色器的列表，以及可选的回退和/或自定义编辑器声明。


##详细信息



###属性

着色器可以具有[属性](SL-Properties.html)列表。着色器中声明的所有属性都会显示在 Unity 中的[材质检视面板](class-Material.html)中。典型的属性包括对象颜色、纹理或者着色器要使用的任意值。


###子着色器和回退

每个着色器均包含一组[子着色器](SL-SubShader.html)。必须至少包含一个子着色器。加载着色器时，Unity 将检查子着色器的列表，并选取最终用户的机器支持的第一个子着色器。如果不支持任何子着色器，Unity 将尝试使用[回退着色器](SL-Fallback.html)。

不同显卡具有不同功能。这给游戏开发者带来了一个永恒的问题；您希望游戏在最新款硬件上呈现出色的画面，但又不希望这些出色画面只能在数量仅占 3% 的最新款硬件上呈现。这时候就需要使用子着色器。可创建一个子着色器，使其包含您所能想象的所有奇幻图形特效，然后为型号更老的显卡添加更多子着色器。这些子着色器可能会以更慢的速度来实现您想要的效果，或者可能选择不实现某些细节。

着色器的“细节级别”(LOD) 和“着色器替换”是两种同样基于子着色器的技术，请参阅[着色器 LOD](SL-ShaderLOD.html) 和[着色器替换](SL-ShaderReplacement.html)以了解想象信息。


##示例



下面是最简单的着色器之一：


````
// 有色顶点光照
Shader "Simple colored lighting"
{
    // 单个颜色属性
    Properties {
        _Color ("Main Color", Color) = (1,.5,.5,1)
    }
    // 定义一个子着色器
    SubShader
    {
        // 子着色器中的单个通道
        Pass
        {
            // 使用固定函数每顶点光照
            Material
            {
                Diffuse [_Color]
            }
            Lighting On
        }
    }
}
````

此着色器定义了颜色属性 __\_Color__（在材质检视面板中显示为 _Main Color_），默认值为 __(1,0.5,0.5,1)__。然后，定义了单个子着色器。该子着色器包含一个 [Pass](SL-Pass.html)，用于启用固定函数顶点光照并为其设置基本材质。

请参阅[表面着色器示例](SL-SurfaceShaderExamples.html)或
[顶点和片元着色器示例](SL-VertexFragmentShaderExamples.html)中提供的更复杂示例。


## 另请参阅

* [Properties 语法](SL-Properties.html)。
* [SubShader 语法](SL-SubShader.html)。
* [Pass 语法](SL-Pass.html)。
* [Fallback 语法](SL-Fallback.html)。
* [CustomEditor 语法](SL-CustomEditor.html)。
