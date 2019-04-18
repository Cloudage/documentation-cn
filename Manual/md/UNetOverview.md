#多人游戏概述

**相关教程**：[多玩家联网 (Multiplayer Networking)](https://unity3d.com/learn/tutorials/topics/multiplayer-networking)

网络功能有两种用户：

* 使用 Unity 开发多人游戏的用户。这些用户应该以 [NetworkManager](UNetManager.html) 或[高级 API](UNetUsingHLAPI.html) 为起点。
* 构建网络基础架构或高级多人游戏的用户。这些用户应该以 [NetworkTransport API](UNetUsingTransport.html) 为起点。

##高级脚本 API

Unity 的网络功能有一个“高级”脚本 API（我们称之为 HLAPI）。使用此 API 意味着可以访问满足多用户游戏大多数常见要求的命令，而无需担心“较低级别”的实现细节。HLAPI 提供以下功能：

* 使用 Network Manager 来控制游戏的联网状态。
* 操作“客户端托管的”游戏，这种情况下的主机也是玩家客户端。
* 使用通用序列化程序来序列化数据。
* 发送和接收网络消息。
* 将联网命令从客户端发送到服务器。
* 执行从服务器到客户端的远程过程调用 (RPC)。
* 将联网事件从服务器发送到客户端。

##引擎和 Editor 集成

Unity 的联网系统集成在引擎和 Editor 中，因此便于使用组件和视觉辅助工具来构建多人游戏。该系统具有以下功能：

* 用于联网对象的 [NetworkIdentity](../ScriptReference/Networking.NetworkIdentity.html) 组件。
* 用于联网脚本的 [NetworkBehaviour](../ScriptReference/Networking.NetworkBehaviour.html)。
* 可配置的对象变换自动同步。
* 脚本变量自动同步。
* 支持在 Unity 场景中放置联网对象。
* [网络组件](UNetReference.html)

##互联网服务

Unity 提供[互联网服务](UnityMultiplayerSettingUp.html)为游戏的整个生产和发布过程提供支持，这些服务包括：

* 配对服务
* 创建比赛和通告比赛。
* 列出可用的比赛和加入比赛。
* 中继服务器 (Relay Server)
* 基于互联网但无专用服务器的游戏。
* 比赛参与者的消息传送。

##NetworkTransport 实时传输层

我们加入了一个具备以下功能的[实时传输层](UNetUsingTransport.html)：

* 基于 UDP 的优化协议。
* 多通道设计可避免线头阻塞问题
* 每个通道支持各种级别的服务质量 (QoS)。
* 灵活的网络拓扑可支持对等架构或客户端/服务器架构。

##示例项目

还可以深入了解我们的多人游戏示例项目，了解这些功能如何配合使用。可在[此 Unity 论坛帖子](http://forum.unity3d.com/threads/unet-sample-projects.331978/)中找到以下示例项目：

* 多人 2D 坦克示例游戏
* 具有配对功能的多人入侵者游戏
* 具有配对功能的多人 2D 太空射击游戏
* 最小的多人游戏项目

