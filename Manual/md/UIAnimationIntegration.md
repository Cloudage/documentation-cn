动画集成
=====================


通过动画可以使用 Unity 的动画系统完全动画化控件状态之间的每个过渡。由于可以同时动画化的属性数量很多，因此这是最强大的过渡模式。

![](../uploads/Main/GUI_ButtonInspectorAnimation.png) 

要使用动画过渡模式，必须将动画器组件附加到控制器元素。通过单击“Auto Generate Animation”即可自动完成此操作。此过程也会生成一个已经设置状态的动画控制器，并需要对此进行保存。

新的动画控制器可以立即使用。与大多数动画控制器不同，此控制器还会存储控制器过渡的动画，如果需要，还可以进行自定义。

![](../uploads/Main/GUI_ButtonAnimator.png) 

例如，如果选择附加了动画控制器的按钮元素，则可以通过打开 Animation 窗口 (**Window&gt;Animation**) 来编辑每个按钮状态的动画。

有一个 Animation Clip 弹出菜单可以选择所需的剪辑。选项包括“Normal”、“Highlighted”、“Pressed”和“Disabled”。

![](../uploads/Main/GUI_ButtonAnimationWindow.png) 

Normal 状态由按钮元素本身上的值设置，并可保留为空。在所有其他状态中，最常见的配置是时间轴开头的单个关键帧。状态之间的过渡动画将由动画器处理。

例如，可从 Animation Clip 弹出菜单中选择 Highlighted 状态并将播放头置于时间轴的开头来更改 Highlighted 状态按钮的宽度：

* 选择录制按钮
* 在 Inspector 中更改按钮的宽度
* 退出录制模式。

切换到播放模式以查看按钮突出显示时的变化情况。

任何数量的属性都可以在这一个关键帧中设置其参数。

通过共享动画控制器，多个按钮可共享相同的行为。

**UI 动画过渡模式与 Unity 的旧版动画系统不兼容。**仅应使用*动画器*组件。

