# 空间映射常见问题故障排除

空间映射会生成大量数据。这会对应用程序的速度和性能产生影响。本部分将讨论使用空间映射时可能产生的常见问题，并提供关于如何避免这些问题的指导。

## 碰撞数据生成需要很长时间

生成碰撞数据比空间映射的任何其他方面需要的 CPU 处理时间都要多。确保仅在必要时请求碰撞数据，这样可以优化 CPU 资源的使用效率并延长电池续航时间。

###最佳实践解决方案：

* 仅在必要时请求碰撞数据。避免在不需要时请求碰撞数据可降低其他空间映射请求的延迟，并延长电池续航时间。

* 使用__表面__的更新时间和包围盒来确定数据请求的优先级，仅请求高优先级的__表面__。

## 较高的三角形密度会产生大量几何体

在使用 [RequestMeshAsync](../ScriptReference/XR.WSA.SurfaceObserver.RequestMeshAsync.html) 方法请求 __Surface__ 数据时，如果通过 [SurfaceData](../ScriptReference/XR.WSA.SurfaceData.html) 结构设置较高的 [trianglesPerCubicMeter](../ScriptReference/XR.WSA.SurfaceData-trianglesPerCubicMeter.html) 值，则会在场景中生成大量的几何体。在包含大量对象的空间（如杂乱的办公室）中尤其如此。场景中的大量几何体会增加数据生成延迟和应用程序内存使用量。场景中较高的网格密度也会降低渲染和物理系统等运行时系统的速度，从而可影响性能。

###最佳实践解决方案：

使用应用程序所需的空间映射数据的最小分辨率。为此，必须测试应用程序并验证生成的空间映射网格的准确性，但结果需要在精度和性能之间取得平衡，最终提供更出色的用户体验。生成的网格的分辨率越低，网格更新时应用程序所使用的 CPU 时间就越少。这样可以降低设备的功耗，延长电池续航时间，还可以降低检索网格数据时的延迟。较低分辨率的网格还使用较少的内存，对渲染和物理系统等运行时系统（期望低复杂度的几何体）的影响也较小。

## 太多网格请求处于排队状态导致不必要的工作

当您调用 [SurfaceObserver.Update](../ScriptReference/XR.WSA.SurfaceObserver.Update.html) 方法时，[SurfaceObserver](../ScriptReference/XR.WSA.SurfaceObserver.html) 会报告所在体积中所有已添加、更新和移除的__表面__。

这样就会将所有已更改的__表面__作为一个列表添加到工作队列中，因此可能导致在空间映射删除这些表面后在工作队列中保留未使用的__表面__。这些__表面__在系统中移动时仍会占用 CPU 时间，但不生成任何网格数据。这会增加所有处于等待状态的请求的延迟时间。

###最佳实践解决方案：

尽可能限制工作队列中的__表面__数量。查询网格是一项高成本、高延迟的操作，因此在任何给定时间只保留一个 [RequestMeshAsync](../ScriptReference/XR.WSA.SurfaceObserver.RequestMeshAsync.html) 请求可以最大限度减少这些操作导致的任何减速问题。应用程序可以使用__表面__报告的更新时间和边界来确定 `RequestMeshAsync` 调用的优先级。

确定__表面__数据请求的优先级也很重要，这样应用程序才能首先收到最重要的数据。确定优先级的常用方法的示例包括：

* 新__表面__的优先级高于更新现有表面。

* 离用户更近的__表面__的优先级高于离用户更远的表面。

## 重叠的 SurfaceObserver 体积会产生重复的 RequestMeshAsync 调用

[SurfaceObserver](../ScriptReference/XR.WSA.SurfaceObserver.html) 会报告与其体积重叠的所有__表面__的更改。一个__表面__可能重叠多个 `SurfaceObserver` 体积（如果它们非常靠近）。因此，应用程序代码可能多次请求相同的__表面__，这可能导致性能问题。

__最佳实践解决方案：__

对来自 `SurfaceObserver` 的请求使用单个工作提交队列。

对于多个应用程序，通常只需要一个 `SurfaceObserver`。

使用单个 `SurfaceObserver` 有助于降低开发应用程序的复杂性。但是，__空间映射__有几种高级用途需要多个 `SurfaceObserver` 成员。在这些情况下，应该为场景中的所有 `SurfaceObserver` 成员使用单个工作队列，从而唯一确定网格请求的优先级。

## 更新 SurfaceObserver 不会生成 onSurfaceChanged 回调

这是一个常见问题，通常由设置过程中的问题引起。

###最佳实践解决方案：

使用 [SetVolumeAsAxisAlignedBox](../ScriptReference/XR.WSA.SurfaceObserver.SetVolumeAsAxisAlignedBox.html) 等方法在 `SurfaceObserver` 上设置有效体积。

还应确保在 __Publishing Settings__（菜单：__File > Build Settings > Player Settings > Publishing Settings__）中启用 __Spatial Perception__ 功能。

有关 WMR 的该功能和发布设置 (Publishing Settings) 的更多信息，请参阅 Unity 的 [Windows Mixed Reality 快速入门指南](wmr_quick_start.html)。

--

* <span class="page-edit">2018-05-01 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 2017.3 版中更新了 Hololens 空间映射文档</span>
