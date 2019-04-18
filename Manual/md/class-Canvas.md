#画布

__画布 (Canvas)__ 组件表示进行 UI 布局和渲染的抽象空间。所有 UI 元素都必须是附加了画布组件的游戏对象的子对象。从菜单 (__GameObject > Create UI__) 创建 UI 元素对象时，如果场景中没有画布 (Canvas) 对象，则会自动创建该对象。

![Screen Space - Overlay 设置](../uploads/Main/UI_CanvasInspector.png)

![Screen Space - Camera 设置](../uploads/Main/UI_CanvasScreenSpaceCameraInspector.png)

![World Space 设置](../uploads/Main/UI_CanvasWorldSpaceInspector.png)

##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Render Mode__ | UI 在屏幕上或作为 3D 空间对象进行渲染的方式（见下文）。提供的选项包括 _Screen Space - Overlay_、_Screen Space - Camera_ 和 _World Space_。 |
|__Pixel Perfect（仅限 _Screen Space_ 模式）__ |是否应该无锯齿精确渲染 UI？ |
|__Render Camera（仅限 _Screen Space - Camera_ 模式）__ |应该将 UI 渲染到的摄像机（见下文）。 |
|__Plane Distance（仅限 _Screen Space - Camera_ 模式）__ |UI 平面在摄像机前方的距离。 |
|__Event Camera（仅限 _World Space_ 模式）__ |用于处理 UI 事件的摄像机。 |
|__Receives Events__ |此画布是否处理 UI 事件？ |


##详细信息

所有 UI 元素使用一个画布就足够了，但场景中可以有多个画布。此外，为了实现优化目的，还可以使用嵌套的画布，使一个画布作为另一个画布的子项。嵌套的画布使用与其父项相同的渲染模式。

传统上，渲染 UI 的效果就好像是直接在屏幕上绘制的简单图形设计。也就是说，没有摄像机观察 3D 空间的概念。Unity 便支持这种屏幕空间渲染方式，但也允许 UI 在场景中渲染为对象，具体取决于 _Render Mode_ 属性的值。可用的模式包括 **Screen Space - Overlay**、**Screen Space - Camera** 和 **World Space**。

###Screen Space - Overlay

在此模式下，画布会进行缩放来适应屏幕，然后直接渲染而不参考场景或摄像机（即使场景中根本没有摄像机，也会渲染 UI）。如果更改屏幕的大小或分辨率，则 UI 将自动重新缩放进行适应。UI 将绘制在所有其他图形（例如摄像机视图）上。

![Overlay UI 渲染在场景对象上](../uploads/Main/CanvasOverlay.png)

注意：Screen Space - Overlay 画布需要存储在层级视图的顶级。如果未使用此设置，则 UI 可能会从视图中消失。这是一项内置的限制。请将 Screen Space - Overlay 画布保持在层级视图的顶级以便获得期望的结果。

###Screen Space - Camera

在此模式下，画布的渲染效果就好像是在摄像机前面一定距离的平面对象上绘制的效果。UI 在屏幕上的大小不随距离而变化，因为 UI 始终会重新缩放来准确适应[摄像机视锥体](FrustumSizeAtDistance.html)。如果更改屏幕的大小或分辨率或更改摄像机视锥体，则 UI 将自动重新缩放进行适应。场景中比 UI 平面更靠近摄像机的所有 3D 对象都将在 UI 前面渲染，而平面后的对象将被遮挡。

![Camera 模式 UI 的前面有场景对象](../uploads/Main/CanvasCamera.png)

###World Space

此模式将 UI 视为场景中的平面对象进行渲染。但是，与 _Screen Space - Camera_ 模式不同，该平面不需要面对摄像机，可以根据喜好任意定向。画布的大小可以使用矩形变换来设置，但画布在屏幕上的大小将取决于摄像机的视角和距离。其他场景对象可以位于画布后面、穿透画布或位于画布前面。

![World Space UI 与场景对象相交](../uploads/Main/CanvasWorldSpace.png)


##提示
* 请在[创建 World Space UI](HOWTO-UIWorldSpace.html) 页面上阅读有关设置世界空间画布 (World Space Canvas) 的更多信息。
* 如需了解如何使画布和 UI 缩放到不同的分辨率或宽高比，请参阅[设计用于多种分辨率的 UI](HOWTO-UIMultiResolution.html) 页面以及[画布缩放器](script-CanvasScaler.html)页面。
