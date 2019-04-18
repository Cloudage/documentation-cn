画布
======
__画布 (Canvas)__ 是应该容纳所有 UI 元素的区域。画布是一种带有画布组件的游戏对象，所有 UI 元素都必须是此类画布的子项。

创建新的 UI 元素（如使用菜单 __GameObject > UI > Image__ 创建图像）时，如果场景中还没有画布，则会自动创建画布。UI 元素将创建为此画布的子项。

画布区域在 Scene 视图中显示为矩形。这样可以轻松定位 UI 元素，而无需始终显示 Game 视图。

__画布__ 使用 EventSystem 对象来协助消息系统。

## 绘制元素的顺序

画布中的 UI 元素按照它们在 Hierarchy 中显示的顺序绘制。首先绘制第一个子项，然后绘制第二个子项，依此类推。如果两个 UI 元素重叠，则后一个元素将显示在前一个元素之上。

要更改元素的显示顺序，只需在 Hierarchy 中拖动元素进行重新排序。也可以通过在变换组件上使用以下方法从脚本控制顺序：SetAsFirstSibling、SetAsLastSibling 和 SetSiblingIndex。


## 渲染模式

画布具有__渲染模式 (Render Mode)__ 设置，可用于在屏幕空间或世界空间中进行渲染。


### Screen Space - Overlay
此渲染模式将 UI 元素放置于在场景之上渲染的屏幕上。如果调整屏幕大小或更改分辨率，则画布将自动更改大小来适应此情况。

![Screen Space Overlay 画布中的 UI](../uploads/Main/GUI_Canvas_Screenspace_Overlay.jpg)


### Screen Space - Camera
此渲染模式类似于 __Screen Space - Overlay__，但在此模式下，画布放置在指定__摄像机__前面的给定距离处。UI 元素由此摄像机渲染，这意味着摄像机设置会影响 UI 的外观。如果摄像机设置为__正交视图__，则 UI 元素将以透视图渲染，透视失真量可由摄像机__视野__控制。如果调整屏幕大小、更改分辨率或摄像机视锥体发生改变，则画布也将自动更改大小来适应此情况。

![Screen Space Camera 画布中的 UI](../uploads/Main/GUI_Canvas_Screenspace_Camera.jpg)


### World Space
在此渲染模式下，画布的行为与场景中的所有其他对象相同。画布大小可用矩形变换进行手动设置，而 UI 元素将基于 3D 位置在场景中的其他对象前面或后面渲染。此模式对于要成为世界一部分的 UI 非常有用。这种界面也称为“叙事界面”。

![World Space 画布中的 UI](../uploads/Main/GUI_Canvas_Worldspace.jpg)

