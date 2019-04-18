#集成 Multiplayer 服务

在项目中可采用三种不同方法开始使用 Multiplayer 服务。这三种方法根据需求提供了不同的控制程度。

- 使用 NetworkManagerHUD。（最简单，不需要编写任何脚本）
- 使用 NetworkServer 和 NetworkClient。（高级，编写比较简单的脚本）
- 直接使用 NetworkTransport。（低级，编写更复杂的脚本）

第一种方法（使用 __NetworkManagerHUD__）提供最高级别的抽象，这意味着该服务将为您完成大部分工作。因此，这是最简单的方法，最适合于首次创建多人游戏的用户。这种方法提供了一个简单图形界面，可用于执行基本的多玩家任务，包括创建、列出、加入和启动游戏（称为“比赛”）。

第二种方法（使用 __NetworkServer__ 和 __NetworkClient__）使用[网络高级 API](UNetUsingHLAPI.html) 来执行这些相同的任务。这种方法更加灵活；您可以使用提供的示例在游戏自身 UI 中集成基本多玩家任务。

第三种方法（直接使用 _NetworkTransport_）可实现最大程度的控制，但仅当您的需求与众不同，无法使用[网络高级 API](UNetUsingHLAPI.html) 实现时，这种方法才有必要。
