#WebGL 中的光标锁定和全屏模式

光标锁定（使用 __[Cursor.lockState](../ScriptReference/Cursor-lockState.html)__）和全屏模式（使用 __[Screen.fullScreen](../ScriptReference/Screen-fullScreen.html)__）均受 Unity WebGL 的支持，这是使用相应的 HTML5 API（Element.requestPointerLock 和 Element.requestFullscreen）实现的。Firefox 和 Chrome 支持这些功能。Safari 目前无法使用全屏和光标锁定。

##启用 WebGL 中的光标锁定和全屏模式

出于安全考虑，浏览器只会允许通过锁定光标或进入全屏模式来直接响应用户发起的事件（如鼠标点击或按键行为）。遗憾的是，Unity 没有单独的事件和渲染循环，因此会将事件处理推迟到以下时间点：浏览器不再确认 Unity 脚本发出的全屏或光标锁定请求（作为对触发该请求的事件的直接响应）。因此，Unity 会在用户发起的下一个事件（而不是触发光标锁定或全屏请求的事件）时触发该请求。

要使此功能实现可接受的结果，应在发生鼠标/按键按下事件而不是鼠标/按键松开事件时触发光标锁定或全屏请求。这样可以确保当请求延迟到用户发起的下一个事件时能够在用户释放鼠标或按键时触发该请求。

如果使用 Unity 的 [UI.Button](../ScriptReference/UI.Button.html) 组件，为了实现所需的行为，可创建 `Button` 的一个子类来重写 [OnPointerDown](../ScriptReference/UI.Selectable.OnPointerDown.html)
 方法。
 
请注意，在进入全屏模式或锁定光标之前，浏览器可能会显示通知消息或询问用户是否允许。
