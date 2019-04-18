#ShaderLab：通道标签

通道使用标签来告知它们期望何时以何种方式被渲染到渲染引擎。


##语法

````
	Tags { "TagName1" = "Value1" "TagName2" = "Value2" }
````
指定 **TagName1** 具有 **Value1**，**TagName2** 具有 **Value2**。可根据需要添加任意数量的标签。


##详细信息


从根本上说，标签就是键/值对。在[通道](SL-Pass.html)中，标签用于控制此通道在光照管线（环境光、顶点光照、像素光照等等）中具有哪个角色以及某些其他选项。请注意，以下由 Unity 识别的标签_必须_位于 Pass 部分中，不能在 SubShader 中！

###LightMode 标签

**LightMode** 标签定义通道在光照管线中的角色。请参阅[渲染管线](SL-RenderPipeline.html)以了解详细信息。很少手动使用这些标签；大部分情况下，需要与光照进行交互的着色器都先编写为[表面着色器](SL-SurfaceShaders.html)，然后再处理这些细节。

LightMode 标签的可能值：

* **Always**：始终渲染；不应用光照。
* **ForwardBase**：在[前向渲染](RenderTech-ForwardRendering.html)中使用；应用环境光、主方向光、顶点/SH 光源和光照贴图。
* **ForwardAdd**：在[前向渲染](RenderTech-ForwardRendering.html)中使用；应用附加的每像素光源（每个光源有一个通道）。
* **Deferred**：在[延迟渲染](RenderTech-DeferredShading.html)中使用；渲染 G 缓冲区。
* **ShadowCaster**：将对象深度渲染到阴影贴图或深度纹理中。
* **MotionVectors**：用于计算每对象运动矢量。
* **PrepassBase**：在[旧版延迟光照](RenderTech-DeferredLighting.html)中使用；渲染法线和镜面反射指数。
* **PrepassFinal**：在[旧版延迟光照](RenderTech-DeferredLighting.html)中使用；通过组合纹理、光照和反光来渲染最终颜色。
* **Vertex**：当对象不进行光照贴图时在[旧版顶点光照渲染](RenderTech-VertexLit.html)中使用；应用所有顶点光源。
* **VertexLMRGBM**：当对象不进行光照贴图时在[旧版顶点光照渲染](RenderTech-VertexLit.html)中使用；在光照贴图为 RGBM 编码的平台上（PC 和游戏主机）。
* **VertexLM**：当对象不进行光照贴图时在[旧版顶点光照渲染](RenderTech-VertexLit.html)中使用；在光照贴图为双 LDR 编码的平台上（移动平台）。


### PassFlags 标签

一个通道可指示一些标志来更改[渲染管线](SL-RenderPipeline.html)向通道传递数据的方式。这可通过使用 **PassFlags** 标签来实现，该标签的值为空格分隔的标志名称。目前支持以下标志：

* **OnlyDirectional**：在 ForwardBase 通道类型中使用时，此标志的作用是仅允许主方向光和环境光/光照探针数据传递到着色器。这意味着非重要光源的数据将*不会*传递到顶点光源或球谐函数着色器变量。请参阅[前向渲染](RenderTech-ForwardRendering.html)以了解详细信息。


###RequireOptions 标签

一个通道可指示仅当满足某些外部条件时才渲染该通道。这可通过使用 **RequireOptions** 标签来实现，该标签的值为空格分隔的选项字符串。目前，Unity 支持以下选项：

* **SoftVegetation**：仅当 [Quality Settings](class-QualitySettings.html) 中开启了 Soft Vegetation 时才渲染此通道。


##另请参阅

也可为子着色器提供标签，请参阅[子着色器标签](SL-SubShaderTags.html)。
