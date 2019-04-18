# 延迟着色渲染路径

本页面将详细介绍延迟着色[渲染路径](RenderingPaths.html)。有关介绍性的技术概述，请参阅 [Wikipedia：延迟着色 (deferred shading)](http://en.wikipedia.org/wiki/Deferred_shading)。

## 概述

使用延迟着色时，可影响游戏对象的光源数量没有限制。所有光源都按像素进行评估，这意味着它们都能与法线贴图等正确交互。此外，所有光源都可以有剪影和阴影。

延迟着色的优点是，光照的处理开销与接受光照的像素数成正比。这取决于场景中的光量大小，而不管接受光照的游戏对象有多少。因此，可通过减少光源数量来提高性能。延迟着色还具有高度一致和可预测的行为。每个光源的效果都是按像素计算的，因此不会有在大三角形上分解的光照计算。

在缺点方面，延迟着色并不支持抗锯齿，也无法处理半透明游戏对象（这些对象使用[前向](RenderTech-ForwardRendering.html)渲染进行渲染）。此外，它也不支持网格渲染器 (Mesh Renderer) 的接受阴影 (Receive Shadows) 标志，并且仅在有限程度上支持剔除遮罩。最多只能使用四个剔除遮罩。也就是说，剔除层遮罩必须至少包含所有层减去四个任意层，即必须设置 32 个层中的 28 个层。否则，会产生图形瑕疵。

## 要求


延迟着色要求显卡具有多渲染目标 (MRT)、着色器模型 3.0（或更高版本）并支持深度渲染纹理。从 GeForce 8xxx、Radeon X2400、Intel G45 开始，2006 年以后制造的大多数 PC 显卡都支持延迟着色。

On mobile, deferred shading is supported on all devices running at least OpenGL ES 3.0.

**注意：**使用正交投影 (Orthographic projection) 时不支持延迟渲染。如果摄像机的投影模式设置为正交模式，则摄像机将回退到前向渲染。

## 性能注意事项


延迟着色中的实时光源的渲染开销与接受光照的像素数成比例，并_不_依赖于场景复杂度。所以小型点光源或聚光灯的渲染成本非常低，如果它们被场景游戏对象完全或部分遮挡，那么成本甚至更低。

当然，有阴影的光源比没有阴影的光源的成本高得多。在延迟着色中，对于每个阴影投射光源，仍然需要将投射阴影的游戏对象渲染一次或多次。此外，应用阴影的光照着色器的渲染开销高于禁用阴影时的渲染开销。


## 实现详细信息


如果对象的着色器不支持延迟着色，则会在延迟着色结束后使用[前向渲染](RenderTech-ForwardRendering.html)路径来渲染这些对象。

下面列出了几何缓冲区（G 缓冲区）中渲染目标 (RT0 - RT4) 的默认布局。数据类型放置在每个渲染目标的各个通道中。使用的通道显示在括号内。

* RT0，ARGB32 格式：漫射颜色 (RGB)，遮挡 (A)。
* RT1，ARGB32 格式：镜面反射颜色 (RGB)，粗糙度 (A)。
* RT2，ARGB2101010 格式：世界空间法线 (RGB)，未使用 (A)。
* RT3，ARGB2101010（非 HDR）或 ARGBHalf (HDR) 格式：发射 + 光照 + 光照贴图 + 反射探针缓冲区。
* 深度+模板缓冲区。

因此，默认的 G 缓冲区布局为 160 位/像素（非 HDR）或 192 位/像素 (HDR)。

如果混合光照模式为 [Shadowmask](LightMode-Mixed-Shadowmask.html) 或 [Distance Shadowmask](LightMode-Mixed-DistanceShadowmask.html)，则使用第五个目标：

*RT4，ARGB32 格式：光照遮挡值 (RGBA)。

因此，G 缓冲区布局为 192 位/像素（非 HDR）或 224 位/像素 (HDR)。

如果硬件不支持五个并发渲染目标，则使用阴影遮罩的对象将回退到前向渲染路径。
当摄像机不使用 HDR 时，发射+光照缓冲区 (RT3) 采用对数编码，因此提供的动态范围高于 ARGB32 纹理通常可能提供的范围。

请注意，当摄像机使用 HDR 渲染时，不会为发射 + 光照缓冲区 (RT3) 创建单独的渲染目标；而是将摄像机渲染到的渲染目标（即传递给图像效果的渲染目标）用作 RT3。


### G 缓冲区通道

G 缓冲区通道将每个游戏对象渲染一次。漫射和镜面反射颜色、表面平滑度、世界空间法线和发射+环境+反射+光照贴图都将渲染到 G 缓冲区纹理中。G 缓冲区纹理设置为全局着色器属性供着色器以后访问 (_CameraGBufferTexture0 .._CameraGBufferTexture3 names)。


### 光照通道

光照通道根据 G 缓冲区和深度来计算光照。光照是在屏幕空间内计算的，因此处理所需的时间与场景复杂性无关。光照将添加到发射缓冲区。

不与摄像机近平面相交的点光源和聚光灯将渲染为 3D 形状，并会启用 Z 缓冲区对场景的测试。因此，部分或完全遮挡的点光源和聚光灯的渲染成本很低。方向光以及与近平面相交的点光源/聚光灯将渲染为全屏四边形。

如果光源启用了阴影，那么也会在此通道中渲染并应用阴影。请注意，阴影并非是“无成本”的；需要渲染阴影投射物，并且必须应用更复杂的光照着色器。

唯一可用的光照模型是标准 (Standard) 光照模型。如果需要不同的模型，可修改光照通道着色器，方法是将[内置着色器](http://unity3d.com/support/resources/assets/built-in-shaders)中的 Internal-DeferredShading.shader 文件的修改版本放入“Assets”文件夹中名为“Resources”的文件夹内。然后，选择 Edit > Project Settings > Graphics 窗口。将“Deferred”下拉选单改为“Custom Shader”。然后，更改当前使用的着色器对应的着色器 (Shader) 选项。

---

* <span class="page-edit"> 2017-06-08  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 5.6 版中添加了光照模式（__Shadowmask__ 和 __Distance Shadowmask__）</span>
