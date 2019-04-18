# 空间映射 (Spatial Mapping) 组件

为了支持 Unity 中的空间映射并轻松访问其功能，Unity 提供了两个空间映射 XR [组件](UsingComponents.html)：

* [空间映射渲染器 (Spatial Mapping Renderer)](#SpatialMappingRenderer)

* [空间映射碰撞体 (Spatial Mapping Collider)](#SpatialMappingCollider)

这些组件可以一起使用或独立使用。每个空间映射组件都使用自己的 `SurfaceObserver` 来了解物理世界的变化。每个空间映射组件定期查询空间映射系统来了解物理环境中发生的变化（执行这些查询的频率取决于您如何配置它们）。当系统向组件通知有相关变化时，组件会确定对各种发生变化的表面进行烘焙的优先级。

烘焙过程涉及生成网格过滤器 (Mesh Filter) 以及对应于物理表面的网格。这些组件以自己的特定方式使用此网格过滤器。

每个空间映射组件独立于其他空间映射组件。这意味着每个组件都维护自己的表面列表，即使多个组件识别到相同的表面也是如此。为了优化性能，建议限制所使用的空间映射组件的数量。

---

* <span class="page-edit">2018-05-01 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 2017.3 版中更新了 Hololens 空间映射文档</span>
