# Network Manager HUD

Network Manager HUD 可以简单快速提供一些基本功能来让游戏玩家托管联网游戏或查找和加入现有联网游戏。该组件显示一组简单的 UI 按钮，当 Editor 处于播放模式时，这些按钮将显示在 Game 视图中。该组件用作有用的短期解决方案，有助于开始快速开发游戏。在准备好后，您应该将其提供的 UI 替换为您自己的 UI，从而更好地适应您的游戏。

有关使用 Network Manager HUD 组件的完整指南以及运行时显示的控件，请参阅[使用 Network Manager HUD](UNetManagerHUD.html.html)。

![Inspector 窗口中的 Network Manager HUD 组件](../uploads/Main/NetworkManagerHUDComponent.png)

|**属性**|**功能**|
|:---|:---|
|**Show Runtime GUI**|勾选此复选框可在运行时显示 Network Manager HUD GUI。这样可以显示或隐藏此 GUI 以便快速调试。|
|**GUI Horizontal Offset**|设置 HUD 的水平像素偏移（从屏幕左边缘开始测量）。|
|**GUI Vertical Offset**|Set the vertical pixel offset of the HUD, measured from the top edge of the screen.|

*注意：***_Network Manager HUD _***设计为临时开发辅助手段。此组件允许您快速运行多人游戏，但在准备就绪之后应将其替换为您自己的 UI 控件。*
