# 使用 Network Manager HUD

注意：本文档假定您已了解基本网络概念，例如主机、服务器和客户端之间的关系。要了解有关这些概念的更多信息，请参阅[网络系统概念](UNetConcepts.html)文档。


![Inspector 中显示的 Network Manager HUD 组件](../uploads/Main/NetworkManagerHUDComponent.png)

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Show Runtime GUI__ | 勾选此复选框可在运行时显示 Network Manager HUD GUI。这样可以显示或隐藏此 GUI 以便快速调试。 |
|__GUI Horizontal Offset__ | 设置 HUD 的水平像素偏移（从屏幕左边缘开始测量）。 |
|__GUI Vertical Offset__ | 设置 HUD 的垂直像素偏移（从屏幕顶部边缘开始测量）。 |


**Network Manager HUD**（“抬头显示”）提供的基本功能可让游戏玩家开始托管联网游戏，或者查找并加入现有的联网游戏。Unity 将 Network Manager HUD 显示为 Game 视图中的简单 UI 按钮的集合。

![Game 视图中显示的 Network Manager HUD UI](../uploads/Main/NetworkManagerHUDUI.png)

Network Manager HUD 是一种快速启动工具，有助于立即开始构建多人游戏，而无需首先构建用于创建/连接/加入游戏的用户界面。使用该组件后，可直接进入游戏编程阶段，然后在开发计划的后期再构建这些控件的自定义版本。

但是，该组件并不适合用在已完成的游戏中。根据该组件的设计意图，这些控件对于开发初期很有用，但在开发后期应创建自定义 UI，让玩家能使用适合具体游戏的界面来查找和加入游戏。例如，您可能希望在风格方面调整屏幕、按钮和可用游戏列表的设计，使之符合游戏的整体风格。

在开始使用 Network Manager HUD 之前，请在场景中创建一个空游戏对象（菜单：**GameObject** > **Create Empty**），并将 Network Manager HUD 组件添加到新游戏对象。

有关 Network Manager HUD Inspector 中显示的属性的说明，请参阅 Network Manager HUD 参考页面。

## 使用 HUD

Network Manager HUD 有两种基本模式：**LAN**（局域网）模式和 **Matchmaker** 模式。这些模式匹配两种常见类型的多人游戏。**LAN** 模式用于创建或加入托管在局域网上的游戏（即连接到同一网络的多台计算机），而 **Matchmaker** 模式用于在互联网上创建、查找和加入游戏（连接到不同网络的多台计算机）。

Network Manager HUD 默认处于 **LAN** 模式，并显示一些与托管和加入 LAN 模式多人游戏相关的按钮。要将 HUD 切换到 **Matchmaker** 模式，请单击 **Enable Match Maker (M)** 按钮。

**注意**：请记住，Network Manager HUD 功能是针对开发的临时辅助功能。此组件允许您快速运行多人游戏，但在准备就绪之后应将其替换为您自己的 UI 控件。
