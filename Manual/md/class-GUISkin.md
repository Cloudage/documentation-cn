GUI Skin（IMGUI 系统）
========


__GUISkin__ 是可应用于 GUI 的 __GUIStyle__ 的集合。每种__控件 (Control)__ 类型都有自己的样式定义。皮肤 (Skin) 的主要目的将样式应用于整个 UI，而不是应用于单独的控件本身。


![Inspector 中显示的 GUI Skin](../uploads/Main/Inspector-GUISkin.png)

要创建 GUISkin，请从菜单栏中选择 __Assets &gt; Create &gt; GUI Skin__。

**请注意**：本页面适用于 [IMGUI](GUIScriptingGuide.html) 系统的一部分；该系统是一个*仅限于脚本*的 UI 系统。Unity 有一个完整的基于游戏对象的 UI 系统，您可能更希望使用该系统。该系统允许在 Scene 视图中以可见对象的形式设计和编辑用户界面元素。请参阅 [UI 系统手册](UISystem.html)以了解更多信息。


属性
----------


GUI Skin 中的所有属性都是单独的 [GUIStyle](class-GUIStyle.html)。请阅读 [GUIStyle](class-GUIStyle.html) 页面了解有关样式 (Style) 用法的更多信息。


|**_属性：_** |**_功能：_** |
|:---|:---|
|__Font__ |用于 GUI 中每个控件的全局字体 |
|__Box__ |用于所有框形的[样式](class-GUIStyle.html) |
|__Button__ |用于所有按钮的[样式](class-GUIStyle.html) |
|__Toggle__ |用于所有开关的[样式](class-GUIStyle.html) |
|__Label__ |用于所有标签的[样式](class-GUIStyle.html) |
|__Text Field__ |用于所有文本字段的[样式](class-GUIStyle.html) |
|__Text Area__ |用于所有文本区域的[样式](class-GUIStyle.html) |
|__Window__ |用于所有窗口的[样式](class-GUIStyle.html) |
|__Horizontal Slider__ |用于所有水平滑动条的[样式](class-GUIStyle.html) |
|__Horizontal Slider Thumb__ |用于所有水平滑动条 Thumb 按钮的[样式](class-GUIStyle.html) |
|__Vertical Slider__ |用于所有垂直滑动条的[样式](class-GUIStyle.html) |
|__Vertical Slider Thumb__ |用于所有垂直滑动条 Thumb 按钮的[样式](class-GUIStyle.html) |
|__Horizontal Scrollbar__ |用于所有水平滚动条的[样式](class-GUIStyle.html) |
|__Horizontal Scrollbar Thumb__ |用于所有水平滚动条 Thumb 按钮的[样式](class-GUIStyle.html) |
|__Horizontal Scrollbar Left Button__ |用于所有水平滚动条向左滚动按钮的[样式](class-GUIStyle.html) |
|__Horizontal Scrollbar Right Button__ |用于所有水平滚动条向右滚动按钮的[样式](class-GUIStyle.html) |
|__Vertical Scrollbar__ |用于所有垂直滚动条的[样式](class-GUIStyle.html) |
|__Vertical Scrollbar Thumb__ |用于所有垂直滚动条 Thumb 按钮的[样式](class-GUIStyle.html) |
|__Vertical Scrollbar Up Button__ |用于所有垂直滚动条向上滚动按钮的[样式](class-GUIStyle.html) |
|__Vertical Scrollbar Down Button__ |用于所有垂直滚动条向下滚动按钮的[样式](class-GUIStyle.html) |
|__Custom 1-20__ |可应用于任何控件的其他自定义样式 |
|__Custom Styles__ |一组可应用于任何控件的其他自定义样式 |
|__Settings__ |整个 GUI 的其他设置 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Double Click Selects Word__ |如果启用此选项，则双击某个单词将选中该单词 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Triple Click Selects Line__ |如果启用此选项，则单击三次某个单词将选中该行 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Cursor Color__ |键盘光标的颜色 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Cursor Flash Speed__ |编辑文本控件时文本光标的闪烁速度 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Selection Color__ |文本所选区域的颜色 |


详细信息
-------


在为游戏创建整个 GUI 时，可能需要为每种不同的控件类型进行大量自定义。在许多不同的游戏类型中，例如实时策略或角色扮演类游戏，实际上需要定义每种单一的控件类型。

因为每个单独的控件都使用特定的样式，所以创建十几个单独的样式并手动分配样式是不合理的做法。GUI Skin 能解决这一问题。通过创建 GUI Skin，可为每个单独的控件设置预定义的样式集合。然后，只需使用一行代码即可应用皮肤 (Skin)，因此无需手动指定每个单独控件的样式。


###创建 GUISkin

GUISkin 是资源文件。要创建 GUI Skin，请从菜单栏中选择 __Assets &gt; Create &gt; GUI Skin__。随后将在 __Project 视图__中加入新的 GUISkin。


![Project 视图中的新 GUISkin 文件](../uploads/Main/GUISkin-ProjectView.png)


###编辑 GUISkin

创建 GUISkin 后，可在 Inspector 中编辑其包含的所有[样式](class-GUIStyle.html)。例如，__Text Field__ [样式](class-GUIStyle.html)将应用于所有文本字段 (Text Field) 控件。


![编辑 GUISkin 中的 Text Field 样式](../uploads/Main/GUISkin-EditingTextField.png)

无论在脚本中创建多少文本字段，这些字段都将使用此[样式](class-GUIStyle.html)。当然，如果愿意，还可以将一个文本字段的样式更改为与另一个文本字段的样式不同。我们稍后将讨论如何执行此操作。


###应用 GUISkin

要将 GUISkin 应用于 GUI，必须使用简单的脚本来读取皮肤并将其应用于控件。


````

	// 创建一个公共变量，我们稍后可向其中分配 GUISkin
		var customSkin : GUISkin;

		// 在 OnGUI() 函数中应用皮肤
		function OnGUI () {
			GUI.skin = customSkin;

			// 现在创建喜欢的任何控件，这些控件将与自定义皮肤一起显示
			GUILayout.Button ("I am a re-Skinned Button");

			// 可为某些控件（但并非所有控件）更改或移除皮肤
			GUI.skin = null;

			// 此处创建的所有控件都将使用默认皮肤而不是自定义皮肤
			GUILayout.Button ("This Button uses the default UnityGUI Skin");
		}


````


在某些情况下，希望两个相同的控件使用不同的样式。为此创建新皮肤并重新分配该皮肤是不合理的。正确的做法应该是在皮肤中使用__自定义__样式。为自定义样式提供一个__名称__；该名称可用作该单独控件的最后一个参数。



````
	// 此皮肤中的一个自定义样式命名为 "MyCustomControl"
		var customSkin : GUISkin;

		function OnGUI () {
			GUI.skin = customSkin;

			// 提供要用作控件函数最后一个参数的样式名称
			GUILayout.Button ("I am a custom styled Button", "MyCustomControl");

			// 也可忽略自定义样式，而使用皮肤的默认按钮样式
			GUILayout.Button ("I am the Skin's Button Style");
		}

````

有关使用 GUIStyle 的更多信息，请阅读 [GUIStyle](class-GUIStyle.html) 页面。有关使用 UnityGUI 的更多信息，请阅读 [GUI 脚本指南](GUIScriptingGuide.html)。
