#滚动条 (Scrollbar)

__滚动条__控件允许用户滚动由于太大而无法完全看到的图像或其他视图。请注意，类似的[滑动条 (Slider)](script-Slider.html)控件用于选择数值而不是滚动。熟悉的示例包括文本编辑器侧面的垂直滚动条以及用于查看大型图像或地图某一部分的一对垂直和水平滚动条。

![滚动条。](../uploads/Main/UI_ScrollbarExample.png)

![](../uploads/Main/UI_ScrollBarInspector.png) 

##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Interactable__ | 此组件是否接受输入？请参阅 [Interactable](script-Selectable.html)。 |
|__Transition__ | 确定控件以何种方式对用户操作进行可视化响应的属性。请参阅[过渡选项](script-SelectableTransition.html)。 |
|__Navigation__ | 确定控件顺序的属性。请参阅[导航选项](script-SelectableNavigation.html)。|
|__Fill Rect__ | 用于控件背景区域的图形。 |
|__Handle Rect__ | 用于控件滑动“控制柄”部分的图形 |
|__Direction__ | 拖动控制柄时滚动条值增加的方向。选项包括 _Left To Right_、_Right To Left_、_Bottom To Top_ 和 _Top To Bottom_。 |
|__Value__ | 滚动条的初始位置值，范围为 0.0 到 1.0。 |
|__Size__ | 控制柄在滚动条内的比例大小，范围为 0.0 到 1.0。 |
|__Number Of Steps__ | 滚动条允许的不同滚动位置的数量。 |

##事件

|**_属性：_** |**_功能：_** |
|:---|:---|
|__On Value Changed__ | 滚动条的当前值发生变化时调用的 [UnityEvent](UnityEvents.html)。该事件可将值作为 `float` 类型动态参数发送。 |


##详细信息

滚动条的值由控制柄沿其长度的位置确定，该值表示为两个端点之间的分数。例如，默认的从左到右滚动条的左端值为 0.0，右端值为 1.0，0.5 表示中间点。通过将 _Direction_ 属性设置为 _Top To Bottom_ 或 _Bottom To Top_，可以垂直定向滚动条。

滚动条和类似的[滑动条](script-Slider.html)控件之间的显著区别在于，滚动条可以改变控制柄大小来表示可用的滚动距离；当视图只能在很短距离内滚动时，控制柄将填充大部分滚动条，并仅允许在任一方向轻微移动。

滚动条有一个名为 _On Value Changed_ 的事件，当用户拖动控制柄时会响应。当前值作为 `float` 参数传递给事件函数。滚动条的典型用例包括：

* 垂直滚动一段文字。
* 水平滚动时间轴。
* 成对使用在水平和垂直方向滚动大型图像以查看缩放的部分。控制柄的大小会发生变化来指示缩放程度，从而表明可滚动的距离。
