# 光照故障排除和性能

可使用以下两种方法之一渲染光照：

* __顶点光照__仅计算网格顶点处的光照，并在表面其余部分插入顶点值。顶点光照不支持某些光照效果，但就处理开销而言，此方法是两种方法中成本较低的方法。另外，此方法可能是旧显卡上唯一可用的方法。

* __像素光照__是在每个屏幕像素处单独计算的。虽然渲染速度较慢，但像素光照却支持一些顶点光照无法实现的效果。仅在使用像素光照的情况下才渲染法线贴图、光照剪影和实时阴影。此外，在像素模式下渲染时，聚光灯形状和点光源高光看起来要好得多。

![聚光灯在像素模式与顶点模式下渲染的比较情况](../uploads/Main/LightPixVertComp.svg)

光照对渲染速度有很大影响，因此必须根据帧率对光照质量进行折衷。由于像素光照比顶点光照具有更高的渲染开销，因此 Unity 仅会将最亮光照部分渲染为每像素质量，而其余部分渲染为顶点光照。在独立构建目标的[质量设置 (Quality Settings)](class-QualitySettings.html) 中可设置像素光照的最大数量。

您可以使用 __Render Mode__ 属性将光照渲染模式设置为像素光照。在决定是否将光照渲染为像素光照时，模式设置为 __Important__ 的光照将被赋予更高的优先级。模式设置为 __Auto__（默认值）后，Unity 将根据给定对象受光照影响的程度自动对光照进行分类。渲染为像素光照的光照是根据逐个对象确定的。

有关更多信息，请参阅关于[优化图形性能](OptimizingGraphicsPerformance.html)的页面。

### Lighting 窗口统计信息

Lighting 窗口的底部将提供统计信息，显示有关运行时性能的重要指标。请参阅有关 [Lighting 窗口](GlobalIllumination.html)的文档以了解更多详细信息。


##阴影性能

实时阴影具有相当高的渲染开销，因此应谨慎使用它们。任何可能投射阴影的对象都必须首先渲染到阴影贴图中，然后该贴图将用于渲染可能接受阴影的对象。与上述的像素/顶点折中相比，启用阴影对性能的影响甚至更大。

柔和阴影比生硬阴影具有更大的渲染开销，但这仅影响 GPU，不会导致额外的 CPU 工作。

[Quality Settings](class-QualitySettings.html) 包含一个 __Shadow Distance__ 值。与摄像机之间的距离超出此值的对象在渲染时完全没有阴影。由于通常情况下也不会注意到远处物体上的阴影，因此这可作为减少必须渲染的阴影数量的有用优化方案。

方向光的一种特殊问题是单个光照可能会照亮整个场景。此问题意味着阴影贴图通常会同时覆盖场景的大部分，这使得阴影容易受到称为“透视锯齿”的问题的影响。简单来说，透视锯齿是指靠近摄像机的阴影贴图像素看起来比那些更远的像素更“大块”。虽然可以增加阴影贴图分辨率以减少此影响，但结果会造成渲染资源浪费，因为远距离区域的阴影贴图在较低分辨率下看起来已经足够良好了。

因此，该问题一个很好的解决方案是使用单独的阴影贴图：当与摄像机之间的距离增加时，阴影贴图的分辨率降低。这些单独的贴图称为__级联 (cascade)__。从 [Quality Settings](class-QualitySettings.html) 中，可选择零级、两级或四级级联；Unity 将计算摄像机视锥体内的级联位置。请注意，级联仅适用于方向光。请参阅[方向光阴影](DirLightShadows.html)页面了解详细信息。


##如何计算阴影贴图的大小

计算贴图大小的第一步是确定光照可以照亮的屏幕视图区域。方向光可以照亮整个屏幕，但对于聚光灯和点光源，该区域是光源边界形状（点光源为球体，聚光灯为锥体）在屏幕上的投影。投影形状在屏幕上具有一定的宽度和高度（以像素为单位）；然后将这两个值中的较大者作为光照的“像素大小”。

当阴影贴图分辨率设置为 __High__ 时（从 [Quality Settings](class-QualitySettings.html) 中进行设置），阴影贴图的大小计算方式如下：

* **方向光**：[NextPowerOfTwo](../ScriptReference/Mathf.NextPowerOfTwo.html) (pixelSize * 1.9)，(像素值* 1.9的结果，然后返回下一个2的幂的值），最大值为 2048。
* **聚光灯**：[NextPowerOfTwo](../ScriptReference/Mathf.NextPowerOfTwo.html) (pixelSize)，(像素值，然后返回下一个2的幂的值），最大值为 1024。
* **点光源**：[NextPowerOfTwo](../ScriptReference/Mathf.NextPowerOfTwo.html) (pixelSize * 0.5)，(像素值* 0.5的结果，然后返回下一个2的幂的值），最大值为 512。

如果显卡有 512MB 或更大的视频内存，方向光的阴影贴图大小上限增加到 4096，聚光灯为 2048，而点光源为 1024。

阴影分辨率设置为 _Medium_ 时，阴影贴图大小是阴影分辨率设置为 __High__ 时的一半，而分辨率设置为 _Low_ 时，阴影贴图大小变为其四分之一。

点光源的大小下限比其他类型的大，因为它们对阴影使用立方体贴图。这意味着此分辨率下的六个立方体贴图面必须同时保存在视频内存中。渲染它们的成本也非常高，因为潜在的阴影投射物可能需要渲染到所有六个立方体贴图面中。


##阴影故障排除

如果您发现一个或多个对象未投射阴影，则应检查以下几点：

* 旧的图形硬件可能不支持阴影。请参阅下面可处理阴影的最低硬件规格列表。

* 可在 [Quality Settings](class-QualitySettings.html) 中禁用阴影。确保启用了正确的质量级别并为该设置启用了阴影。

* 场景中的所有[网格渲染器](class-MeshRenderer.html)都必须正确设置相应的 _Receive Shadows_ 和 _Cast Shadows_。默认情况下两者都已启用，但请检查确保未意外禁用它们。

* 只有不透明对象才投射和接受阴影，因此使用内置[透明](shader-TransparentFamily.html)着色器或粒子着色器的对象既不会投射也不会接受阴影。通常，您可以使用[透明镂空](shader-TransparentCutoutFamily.html)着色器代替具有“间隙”的对象，例如栅栏、植被等。自定义[着色器](Shaders.html)必须采用像素光照并使用[几何渲染队列](SL-SubShaderTags.html)。

* 使用__顶点光照 (VertexLit)__ 着色器的对象不能接受阴影，但可投射阴影。

* 使用[前向渲染路径](RenderTech-ForwardRendering.html)，有些着色器只允许最亮的方向光投射阴影（这种情况尤其发生在 Unity 4.x 版本的旧版内置着色器中）。如果想使用多个阴影投射光，则应改用[延迟着色 (Deferred Shading)](RenderTech-DeferredShading.html) 渲染路径。您可以使用 `fullforwardshadows` [表面着色器](SL-SurfaceShaders.html) 指令启用您自己的着色器以支持“全阴影”。


##阴影的硬件支持

内置阴影几乎适用于 Unity 支持的所有设备。每个平台都支持以下卡：

### PC (Windows/Mac/Linux)

* 通常，所有 GPU 都支持阴影。某些非常旧的 GPU（例如，2005 年制造的 Intel GPU）可能存在例外。

### 移动端
* iPhone 4 不支持阴影。从 iPhone 4S 和 iPad 2 开始的所有后续型号都支持阴影。
* Android：需要 Android 4.0 或更高版本以及 `GL_OES_depth_texture` 支持。最值得注意的是，一些基于 Android Tegra 2/3 的 Android 设备不符合此要求，因此它们不支持阴影。
* Windows Phone：仅 DX11-class GPU (Adreno 4xx/5xx) 支持阴影。

### 游戏主机
* 所有游戏主机都支持阴影。

---

* <span class="page-edit"> 2017-06-08  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 5.6 版中添加了 Lighting 窗口统计信息</span>
