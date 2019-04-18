Unity 的渲染管线
==========================


着色器定义了两点：对象自身的外观（其材质属性）；对象对光照的反应。由于光照计算必须内置在着色器中，并且有很多可能的光源和阴影类型，因此编写“好用”的高质量着色器将是一项复杂的任务。为了简化这一任务，Unity 提供了[表面着色器](SL-SurfaceShaders.html)；在表面着色器中，所有光照、阴影、光照贴图、前向渲染和延迟渲染任务均自动完成。

本文档将介绍 Unity 的光照和渲染管线的特质以及[表面着色器](SL-SurfaceShaders.html)的内部原理。


渲染路径
---------------


如何应用光照以及使用着色器的哪些[通道](SL-Pass.html)取决于使用的[渲染路径](RenderingPaths.html)。着色器中的每个通道均通过[通道标签](SL-PassTags.html)来表达其光照类型。

* 在[前向渲染](RenderTech-ForwardRendering.html)中，将使用 `ForwardBase` 和 `ForwardAdd` 通道。
* 在[延迟着色](RenderTech-DeferredShading.html)中，将使用 `Deferred` 通道。
* 在[旧版延迟光照](RenderTech-DeferredLighting.html)中，将使用 `PrepassBase` 和 `PrepassFinal` 通道。
* 在[旧版顶点光照](RenderTech-VertexLit.html)中，将使用 `Vertex`、`VertexLMRGBM` 和 `VertexLM` 通道。
* 在上述任何情况中，要渲染[阴影](ShadowOverview.html)或深度纹理，都将使用 `ShadowCaster` 通道。



前向渲染路径
----------------------


`ForwardBase` 通道可一次性渲染环境光、光照贴图、主方向光和不重要的（顶点/SH）光源。`ForwardAdd` 通道用于任何附加的每像素光源；针对此类光源照亮的每个对象进行一次调用。请参阅[前向渲染](RenderTech-ForwardRendering.html)以了解详细信息。

如果使用前向渲染，但着色器没有适合前向渲染的通道（即，`ForwardBase` 和 `ForwardAdd` 通道类型均不存在），则会按照在顶点光照通道中的方式来渲染该对象，请参阅下文。


延迟着色路径
---------------------

`Deferred` 通道将渲染光照需要的所有信息（在内置着色器中：漫射颜色、镜面反射颜色、平滑度、
世界空间法线、发光）。它还在发光通道中增加光照贴图、反射探针和环境光照。有关详细信息，请参阅[延迟着色](RenderTech-DeferredShading.html)。


旧版延迟光照路径
-----------------------------


`PrepassBase` 通道将渲染法线和镜面反射指数；`PrepassFinal` 通道将通过组合纹理、光照和发光材质属性来渲染最终颜色。所有常规的场景内光照都在屏幕空间中单独完成。有关详细信息，请参阅[延迟光照](RenderTech-DeferredLighting.html)。



旧版顶点光照渲染路径
--------------------------------


由于顶点光照最常用于不支持可编程着色器的平台，因此 Unity 无法在内部创建多个着色器变体来处理光照贴图与非光照贴图的情况。所以，要处理光照贴图对象和非光照贴图对象，必须显式编写多个通道。

* `Vertex` 通道用于非光照贴图对象。所有光源均通过固定函数 OpenGL/Direct3D 光照模型 ([Blinn-Phong](http://en.wikipedia.org/wiki/Blinn-Phong_shading)) 立即渲染
* `VertexLMRGBM` 通道在光照贴图为 RGBM 编码（PC 和游戏主机）时用于光照贴图对象。情况下不会应用实时光照；通道应该将纹理与光照贴图组合。
* `VertexLMM` 通道在光照贴图为双 LDR 编码（移动平台）时用于光照贴图对象。情况下不会应用实时光照；通道应该将纹理与光照贴图组合。


## 另请参阅

* [图形命令缓冲区](GraphicsCommandBuffers.html)，了解如何扩展 Unity 的渲染管线。
