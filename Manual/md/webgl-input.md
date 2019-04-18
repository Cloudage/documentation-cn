#WebGL 中的输入

##游戏手柄和游戏杆支持

在支持 HTML5 游戏手柄 API 的浏览器上，WebGL 支持游戏手柄和游戏杆（使用 __[Input](../ScriptReference/Input.html)__ 类）。请查看[浏览器兼容性表](webgl-browsercompatibility.html)以了解具体是哪些浏览器。

请注意，一旦用户在内容处于焦点状态时与设备进行交互，浏览器可能只允许访问可用的输入设备。这是一种防止将连接的设备用于浏览器指纹识别的安全措施。因此，在检查 __[Input.GetJoystickNames()](../ScriptReference/Input.GetJoystickNames.html)__ 以查找连接的设备之前，务必指示用户点击其设备上的按钮。

##触控支持

虽然 Unity WebGL [尚未正式支持](webgl-browsercompatibility.html)移动设备，但 __[Input.touches](../ScriptReference/Input-touches.html)__ 和相关 API 已在具有触控支持的浏览器和设备上实现，并且还实现了 __[Input.acceleration](../ScriptReference/Input-acceleration.html)__。

##键盘输入和焦点处理

默认情况下，无论 WebGL 画布是否具有焦点，Unity WebGL 都会处理发送到页面的所有键盘输入。这样做是为了让用户能够立即开始玩基于键盘的游戏，而无需先点击画布进行聚焦。但是，如果页面上有其他 HTML 元素应该接收键盘输入（例如文本字段），这会导致问题，因为 Unity 会在页面的其余部分获取输入事件之前占用输入事件。如果需要让其他 HTML 元素接收键盘输入，可使用 __WebGLInput.captureAllKeyboardInput__ 属性更改此行为。
