# LOD（细节级别）和实时 GI（全局光照）

如果您的模型使用了 [LOD（细节级别）](LevelOfDetail.html)功能，那么在对模型使用实时全局光照 (GI) 之前，请先阅读本文。

在支持烘焙光照和实时 GI 的场景中使用 Unity 的 LOD 系统时，该系统会将细节级别组 (LOD Group) 中细节级别最高的模型像普通静态模型一样照亮。系统采用光照贴图来进行直接光照和间接光照，并对实时 GI 采用不同的光照贴图。

为了让烘焙系统生成实时或烘焙光照贴图，请选择所需的游戏对象，在 Inspector 窗口中查看其渲染器组件，然后确保已启用 __Lightmap Static__。

对于 LOD 组中较低的 LOD，只能将烘焙光照贴图与[光照探针 (Light Probes)](LightProbes.html) 或[光照探针代理体 (Light Probe Proxy Volumes)](class-LightProbeProxyVolume.html) 中的实时 GI 进行组合，而且必须将光照探针或光照探针代理体放置在 LOD 组周围。

无论何时使用实时 GI，即使启用了 __Lightmap Static__，渲染器也会为较低的 LOD 启用 __Light Probes__ 选项。

![这段动画显示了实时环境颜色如何影响较低 LOD 使用的实时 GI](../uploads/Main/LODRealtimeGI.gif)

---

* <span class="page-edit">2017-10-25  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 版中添加了实时全局光照 <span class="search-words">NewIn20173</span></span>
