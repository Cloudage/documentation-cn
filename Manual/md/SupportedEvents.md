支持的事件
=================
事件系统支持许多事件，并可在用户编写的自定义输入模块中进一步自定义它们。

独立输入模块和触摸输入模块支持的事件由接口提供，通过实现该接口即可在 MonoBehaviour 上实现这些事件。如果配置了有效的事件系统，则会在正确的时间调用事件。

- IPointerEnterHandler - OnPointerEnter - 当指针进入对象时调用
- IPointerExitHandler - OnPointerExit - 当指针退出对象时调用
- IPointerDownHandler - OnPointerDown - 在对象上按下指针时调用
- IPointerUpHandler - OnPointerUp - 松开指针时调用（在指针正在点击的游戏对象上调用）
- IPointerClickHandler - OnPointerClick - 在同一对象上按下再松开指针时调用
- IInitializePotentialDragHandler - OnInitializePotentialDrag - 在找到拖动目标时调用，可用于初始化值
- IBeginDragHandler - OnBeginDrag - 即将开始拖动时在拖动对象上调用
- IDragHandler - OnDrag - 发生拖动时在拖动对象上调用
- IEndDragHandler - OnEndDrag - 拖动完成时在拖动对象上调用
- IDropHandler - OnDrop - 在拖动目标对象上调用
- IScrollHandler - OnScroll - 当鼠标滚轮滚动时调用
- IUpdateSelectedHandler - OnUpdateSelected - 每次勾选时在选定对象上调用
- ISelectHandler - OnSelect - 当对象成为选定对象时调用
- IDeselectHandler - OnDeselect - 取消选择选定对象时调用
- IMoveHandler - OnMove - 发生移动事件（上、下、左、右等）时调用
- ISubmitHandler - OnSubmit - 按下 Submit 按钮时调用
- ICancelHandler - OnCancel - 按下 Cancel 按钮时调用
