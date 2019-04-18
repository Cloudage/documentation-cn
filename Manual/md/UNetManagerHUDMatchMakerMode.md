# Matchmaker 模式下的 Network Manager HUD

![Matchmaker 模式下的 Network Manager HUD](../uploads/Main/NetworkManagerHUDMMMode.png)

Matchmaker 模式提供了一个简单的界面让玩家创建、查找和加入 Unity Multiplayer 服务上托管的比赛。

“比赛”（有时也称为游戏会话或游戏实例）是由 Unity Multiplayer 服务托管的游戏的独特实例。使用 Unity Multiplayer 服务，一定数量的玩家可以加入并一起玩游戏。如果有很多玩家玩游戏，可能会有多个比赛，每个比赛都有多个玩家一起玩。

为了使用 Matchmaker 模式，必须首先为项目启用 Unity Multiplayer 服务。为项目启用 Unity Multiplayer 服务后，可使用 Matchmaker 模式下的 HUD 创建或连接到互联网上托管的游戏实例（有时也称为“比赛”或“会话”）。

### **Create Internet Match**

单击 **Create Internet Match** 可开始新的比赛。Unity 的 Multiplayer 服务会创建一个新的游戏实例（“比赛”），然后其他玩家可以找到并加入该游戏实例。

### Find Internet Match

单击 **Find Internet Match** 可向 Unity Multiplayer 服务发送请求。Unity Multiplayer 服务会返回此游戏当前存在的所有比赛的列表。

例如，如果两个单独的玩家连接，并分别创建一个名为“Match A”和“Match B”的比赛，当第三个玩家连接并按下“Find Internet Match”按钮时，“Match A”和“Match B”将显示在可加入的比赛列表中。

在 Network Manager HUD 中，可用的比赛显示为一系列按钮，并显示文本“Join Match: *match name*”（其中 *match name* 是创建比赛的玩家选择的名称）。

![单击 ***_Find Internet Match_*** 后显示的结果示例。在此示例中列出了四个可加入的现有比赛。](../uploads/Main/NetworkManagerHUDMatchList.png)

要加入其中一个可用的比赛，请单击该比赛的“Join Match: *match name*”按钮。或者，单击 **Back to Match Menu** 返回到 Matchmaker 菜单。

将 HUD 替换为自己的 UI 时，可使用更好的方法列出可用的比赛。许多的多人游戏采用可滚动列表显示可用比赛。有时可能希望列表中的每个条目显示比赛名称、玩家当前数量和最大数量以及其他信息，如比赛模式类型 - 如果决定让游戏具有不同的比赛模式（例如“夺旗”、“1 对 1”或“合作”）。

**注意**：有一些特殊字符，如果在比赛名称中使用这些字符，Network Manager HUD 中的可用比赛列表中会修改这些字符。这些字符包括：

* 左方括号：**[**

* 百分比符号：**%**

* 下划线：**_**

如果比赛名称包含这些字符，可用比赛列表中会用方括号将这些字符括起来。所以，名为“my_game”的比赛将在列表中显示为“my[_]game”。

### Change MM Server

此按钮专为 Unity 工程师内部使用而设计（用于测试 Multiplayer 服务）。此处显示的按钮将三个预定义 URL 之一分配给 Network Manager 中的 **MatchMaker Host URI** 字段 -“local”、“staging”和“internet”。但是，**“local”和“staging”**选项*仅供 Unity 工程师内部使用，不适用于一般用途*。

如果选择“local”或“staging”选项，则游戏无法连接到 Unity 的多用户服务。因此，应始终确保此选项设置为“internet”（这是默认设置）。

### MM Uri Display

此处显示当前的 Matchmaker URI（统一资源标识符，用于标识作用的字符串）。要在 Inspector 中查看 URI，请导航到 Network Manager 组件并查看 **MatchMaker Host URI** 字段**。**默认情况下，此字段指向全局 Unity Multiplayer 服务，用于使用 Unity Multiplayer 服务的普通多人游戏。不需要更改此字段。Unity Multiplayer 服务自动将游戏玩家分组到全球的区域服务器。这种分组方式确保了在多人游戏中同一地区的玩家之间实现快速响应，因此，来自欧洲、美国和亚洲的玩家通常最终会与来自同一区域的其他玩家一起玩游戏。

如果要显式控制游戏连接到的区域服务器，请通过编写脚本来重写此值。要了解更多信息和区域服务器 URI，请参阅有关 [NetworkMatch.baseUri](https://docs.unity3d.com/ScriptReference/Networking.Match.NetworkMatch-baseUri.html) 的 API 参考文档。

例如，如果想让玩家选择加入所在区域之外的服务器，可能需要重写 URI。如果美国的“玩家 A”想要连接到由欧洲“玩家 B”通过 Matchmaker 创建的比赛，他们需要能够在游戏中设置所需的区域。因此，您需要编写一个 UI 功能，允许玩家选择此设置。

**注意**：请记住，Network Manager HUD 功能是针对开发的临时辅助功能。此组件允许您快速运行多人游戏，但在准备就绪之后应将其替换为您自己的 UI 控件。



