# 编写表面着色器

编写与光照交互的着色器非常复杂。有不同的光源类型，不同的阴影选项，不同的渲染路径（前向和延迟渲染）；着色器应该以某种方式应对所有这些复杂性。

Unity 中的__表面着色器__是一种代码生成方法，与使用低级[顶点/像素着色器程序](SL-ShaderPrograms.html)相比，可以更轻松地编写光照着色器。请注意，表面着色器中没有自定义的语言、魔法或忍者；它只能生成所有那些本来必须要手工编写的重复代码。您仍然需要使用 HLSL 编写着色器代码。

如需了解示例，请查看[表面着色器示例](SL-SurfaceShaderExamples.html)和[表面着色器自定义光照示例](SL-SurfaceShaderLightingExamples.html)。

## 工作原理

您可以定义一个“表面函数”，它将您需要的所有 UV 或数据作为输入，并填充输出结构 `SurfaceOutput`。SurfaceOutput 基本上描述了_表面的属性_（反照率颜色、法线、发光、镜面反射等）。您需要使用 HLSL 编写此代码。

表面着色器编译器随后计算出需要的输入、填充的输出等等，并生成实际的[顶点和像素着色器](SL-ShaderPrograms.html)以及渲染通道来处理前向和延迟渲染。

以下是表面着色器的标准输出结构：

```
struct SurfaceOutput
{
    fixed3 Albedo;	// 漫射颜色
    fixed3 Normal;	// 切线空间法线（如果已写入）
    fixed3 Emission;
    half Specular;	// 0..1 范围内的镜面反射能力
    fixed Gloss;	// 镜面反射强度
    fixed Alpha;	// 透明度 Alpha
};
```

在 Unity 5 中，表面着色器还可以使用基于物理的光照模型。内置标准光照模型和标准镜面反射光照模型（见下文）分别使用以下输出结构：

```
struct SurfaceOutputStandard
{
	fixed3 Albedo;		// 基础（漫射或镜面反射）颜色
	fixed3 Normal;		// 切线空间法线（如果已写入）
	half3 Emission;
	half Metallic;		// 0=非金属，1=金属
	half Smoothness;	// 0=粗糙，1=平滑
	half Occlusion;		// 遮挡（默认为 1）
	fixed Alpha;		// 透明度 Alpha
};
struct SurfaceOutputStandardSpecular
{
	fixed3 Albedo;		// 漫射颜色
	fixed3 Specular;	// 镜面反射颜色
	fixed3 Normal;		// 切线空间法线（如果已写入）
	half3 Emission;
	half Smoothness;	// 0=粗糙，1=平滑
	half Occlusion;		// 遮挡（默认为 1）
	fixed Alpha;		// 透明度 Alpha
};
```

## 示例


请参阅[表面着色器示例](SL-SurfaceShaderExamples.html)、[表面着色器自定义光照示例](SL-SurfaceShaderLightingExamples.html)和[表面着色器曲面细分](SL-SurfaceShaderTessellation.html)页面。


## 表面着色器编译指令


就像任何其他着色器一样，表面着色器放置在 `CGPROGRAM..ENDCG` 代码块内。不同之处在于：

* 它必须放在 [SubShader](SL-SubShader.html) 代码块内，不能在 [Pass](SL-Pass.html) 内。表面着色器本身将编译为多个通道。
* 它使用 `#pragma surface ...` 指令来指示自己是表面着色器。

`#pragma surface` 指令为：

```
# pragma surface surfaceFunction lightModel [optionalparams]
```


### 必需参数

* `surfaceFunction` - 具有表面着色器代码的 Cg 函数。该函数的格式应为 `void surf (Input IN, inout SurfaceOutput o)`，其中 Input 是您定义的结构。Input 应包含表面函数所需的任何纹理坐标和额外自动变量。
* `lightModel` - 要使用的光照模型。内置光照模型是基于物理的 `Standard` 和 `StandardSpecular`，以及简单的非基于物理的 `Lambert`（漫射）和 `BlinnPhong`（镜面反射）。请参阅[自定义光照模型](SL-SurfaceShaderLighting.html)页面以了解如何编写自己的光照模型。
	* `Standard` 光照模型使用 `SurfaceOutputStandard` 输出结构，并与 Unity 中的标准（金属性工作流）着色器匹配。
	* `StandardSpecular` 光照模型使用 `SurfaceOutputStandardSpecular` 输出结构，并与 Unity 中的标准（镜面反射设置）着色器匹配。
	* `Lambert` 和 `BlinnPhong` 光照模型不是基于物理的（来自 Unity 4.x），但使用这两个光照模型的着色器在低端硬件上可以提高渲染速度。


### 可选参数

**透明度和 Alpha 测试**由 `alpha` 和 `alphatest` 指令控制。透明度通常可以有两种：传统的 Alpha 混合（用于淡出对象）或更符合物理规律的“预乘混合”（允许半透明表面保留适当的镜面反射）。启用半透明度会使生成的表面着色器代码包含[混合](SL-Blend.html)命令；而启用 Alpha 镂空将根据给定的变量在生成的像素着色器中执行片元废弃。

* `alpha` 或 `alpha:auto` - 对于简单的光照函数，将选择淡化透明度（与 `alpha:fade` 相同）；对于基于物理的光照函数，将选择预乘透明度（与 `alpha:premul` 相同）。
* `alpha:blend` - 启用 Alpha 混合。
* `alpha:fade` - 启用传统淡化透明度。
* `alpha:premul` - 启用预乘 Alpha 透明度。
* `alphatest:VariableName` - 启用 Alpha 镂空透明度。剪切值位于具有 VariableName 的浮点变量中。您可能还想使用 `addshadow` 指令生成正确的阴影投射物通道。
* `keepalpha` - 默认情况下，无论输出结构的 Alpha 输出是什么，或者光照函数返回什么，不透明表面着色器都将 1.0（白色）写入 Alpha 通道。使用此选项可以保持光照函数的 Alpha 值，即使对于不透明的表面着色器也是如此。
* `decal:add` - 附加贴花着色器（例如 terrain AddPass）。这适用于位于其他表面之上并使用附加混合的对象。请参阅[表面着色器示例](SL-SurfaceShaderExamples.html)
* `decal:blend` - 半透明贴花着色器。这适用于位于其他表面之上并使用 Alpha 混合的对象。请参阅[表面着色器示例](SL-SurfaceShaderExamples.html)


**自定义修改器函数**可用于更改或计算传入的顶点数据，或更改最终计算的片元颜色。

* `vertex:VertexFunction` - 自定义顶点修改函数。在生成的顶点着色器的开始处调用此函数，并且此函数可以修改或计算每顶点数据。请参阅[表面着色器示例](SL-SurfaceShaderExamples.html)。
* `finalcolor:ColorFunction` - 自定义最终颜色修改函数。请参阅[表面着色器示例](SL-SurfaceShaderExamples.html)。
* `finalgbuffer:ColorFunction` - 用于更改 G 缓冲区内容的自定义延迟路径。
* `finalprepass:ColorFunction` - 自定义预通道基本路径。

**阴影和曲面细分** - 可以提供其他指令来控制阴影和曲面细分的处理方式。

* `addshadow` - 生成阴影投射物通道。常用于自定义的顶点修改，以便阴影投射也可以获得程序化顶点动画。通常情况下，着色器不需要任何特殊的阴影处理，因为它们可以通过回退机制来使用阴影投射物通道。
* `fullforwardshadows` - 支持[前向](RenderTech-ForwardRendering.html)渲染路径中的所有光源阴影类型。默认情况下，着色器仅支持前向渲染中来自一个方向光的阴影（以节省内部着色器变体数量）。如果在前向渲染中需要点光源阴影或聚光灯阴影，请使用此指令。
* `tessellate:TessFunction` - 使用 DX11 GPU 曲面细分；该函数计算曲面细分因子。有关详细信息，请参阅[表面着色器曲面细分](SL-SurfaceShaderTessellation.html)。


**代码生成选项** - 默认情况下，生成的表面着色器代码会尝试处理所有可能的光照/阴影/光照贴图情况。但是在某些情况下，您知道您不需要其中的一部分，可以调整生成的代码以跳过它们。这样可以减小着色器，从而提高加载速度。

* `exclude_path:deferred`、`exclude_path:forward` 和 `exclude_path:prepass` - 不为给定的渲染路径（分别对应[延迟着色](RenderTech-DeferredShading.html)路径、[前向](RenderTech-ForwardRendering.html)路径和[旧版延迟](RenderTech-DeferredLighting.html)路径）生成通道。
* `noshadow` - 禁用此着色器中的所有阴影接受支持。
* `noambient` - 不应用任何环境光照或光照探针。
* `novertexlights` - 在前向渲染中不应用任何光照探针或每顶点光源。
* `nolightmap` - 禁用此着色器中的所有光照贴图支持。
* `nodynlightmap` - 禁用此着色器中的运行时动态全局光照支持。
* `nodirlightmap` - 禁用此着色器中的方向光照贴图支持。
* `nofog` - 禁用所有内置雾效支持。
* `nometa` - 不生成“Meta”通道（由光照贴图和动态全局光照用于提取表面信息）。
* `noforwardadd` - 禁用[前向](RenderTech-ForwardRendering.html)渲染附加通道。这会使着色器支持一个完整方向光，所有其他光源均进行每顶点/SH 计算。也能减小着色器。
* `nolppv` - 禁用此着色器中的光照探针代理体支持。
* `noshadowmask` - 为此着色器禁用阴影遮罩支持（包括 [Shadowmask](LightMode-Mixed-Shadowmask.html) 和 [Distance Shadowmask](LightMode-Mixed-DistanceShadowmask.html)）。


**其他选项**

* `softvegetation` - 仅在开启 Soft Vegetation 时才渲染表面着色器。
* `interpolateview` - 在顶点着色器中计算视图方向并进行插值；而不是在像素着色器中计算。这可以使像素着色器更快，但会额外消耗一个纹理插值器。
* `halfasview` - 将半方向矢量传入光照函数而不是视图方向。计算半方向并按每个顶点对其进行标准化。这更快，但并不完全正确。
* `approxview` - 在 Unity 5.0 中已删除。请改用 `interpolateview`。
* `dualforward` - 在[前向](RenderTech-ForwardRendering.html)渲染路径中使用[双光照贴图](GIIntro.html)。
* `dithercrossfade` - 使表面着色器支持抖动效果。然后，可将此着色器应用于使用[细节级别组 (LOD Group)](class-LODGroup.html) 组件（配置为交叉淡入淡出过渡模式）的游戏对象。


要了解与上述不同选项的具体区别，使用[着色器检视面板 (Shader Inspector)](class-Shader.html) 中的“Show Generated Code”按钮会很有帮助。

## 表面着色器输入结构

输入结构 `Input` 通常具有着色器所需的所有纹理坐标。纹理坐标必须命名为“`uv`”后跟纹理名称的形式（如果要使用第二个纹理坐标集，则以“`uv2`”开头）。

可以放入输入结构的其他值：

* `float3 viewDir` - 包含视图方向，用于计算视差效果、边缘光照等等。
* 具有 `COLOR` 语义的 `float4` - 包含插值的每顶点颜色。
* `float4 screenPos` - 包含反射或屏幕空间效果的屏幕空间位置。请注意，这不适合 [GrabPass](SL-GrabPass.html)；您需要使用 `ComputeGrabScreenPos` 函数自己计算自定义 UV。
* `float3 worldPos` - 包含世界空间位置。
* `float3 worldRefl` - 在_表面着色器不写入 o.Normal_ 的情况下，包含世界反射矢量。有关示例，请参阅反光漫射 (Reflect-Diffuse) 着色器。
* `float3 worldNormal` - 在_表面着色器不写入 o.Normal_ 的情况下，包含世界法线矢量。
* `float3 worldRefl; INTERNAL_DATA` - 在_表面着色器写入 o.Normal_ 的情况下，包含世界反射矢量。要获得基于每像素法线贴图的反射矢量，请使用 `WorldReflectionVector (IN, o.Normal)`。有关示例，请参阅反光凹凸 (Reflect-Bumped) 着色器。
* `float3 worldNormal; INTERNAL_DATA` - 在_表面着色器写入 o.Normal_ 的情况下，包含世界法线矢量。要获得基于每像素法线贴图的法线矢量，请使用 `WorldNormalVector (IN, o.Normal)`。

## 表面着色器和 DirectX 11 HLSL 语法

目前，表面着色器编译管线的某些部分不能理解 [DirectX 11](UsingDX11GL3Features.html) 特有的 HLSL 语法，因此如果您正在使用 HLSL 功能，如 StructuredBuffers、RWTextures 和其他非 DX9 语法，需要将其包装到仅限 DX11 的预处理器宏中。

有关详细信息，请参阅[平台特定差异](SL-PlatformDifferences.html)和[着色语言](SL-ShadingLanguage.html)页面。

---

* <span class="page-edit"> 2017-06-08  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 5.6 版中添加了 `noshadowmask`</span>
