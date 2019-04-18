# 主机迁移

在没有专用服务器的多人网络游戏中，由其中一个游戏实例充当主机，即游戏的授权中心。此玩家的主机同时游戏服务器和“本地客户端”，而其他玩家各自运行“远程客户端”。有关更多信息，请参阅[网络系统概念](UNetConcepts.html)文档。

如果主机断开与游戏的连接，则游戏将无法继续。主机断开连接的常见原因包括主机玩家离开、主机进程崩溃、主机关机或主机中断网络连接。

**主机迁移**功能可让其中一个远程客户端成为新主机，使多人游戏能够继续。

## 工作原理

在启用主机迁移的多人游戏中，Unity 会将所有对等方（即玩家，包括主机和所有客户端）的地址分发给游戏中的所有其他对等方。当主机断开连接时，一个对等方成为新主机。然后，其他对等方连接到新主机，游戏得以继续。

Network Migration Manager 组件使用 Unity 网络 HLAPI。该组件可让游戏在原始主机断开连接后继续使用新主机。下面的截屏显示了 Network Migration Manager 的 Inspector 窗口中显示的迁移状态。

![Network Migration Manager 组件](../uploads/Main/NetworkMigrationManagerInspector.png)

Network Migration Manager 提供一个类似于 [Network Manager HUD](UNetManager.html) 的基本用户界面。该用户界面用于游戏开发期间的测试和原型设计；在发布游戏之前，应实现一个用于主机迁移的自定义用户界面，并为某些操作实现自定义逻辑（比如自动选择新主机，而不需要用户输入）。

![Network Migration Manager 原型设计 HUD](../uploads/Main/NetworkMigrationManagerHUD.png)

即使由于旧主机中断连接或退出游戏而可能发生迁移，但游戏的旧主机可能作为新主机上的客户端重新加入游戏。

在主机迁移期间，Unity 在场景中的所有联网游戏对象上维护 [SyncVar](../ScriptReference/Networking.SyncVarAttribute.html) 和 [SyncList](../ScriptReference/Networking.SyncList_1.html) 的状态。游戏对象的自定义序列化数据也是如此。

当主机断开连接时，Unity 会禁用游戏中的所有玩家游戏对象。然后，当其他客户端在新主机上重新加入新游戏时，相应的玩家单元在新主机上重新启用这些客户端，并在其他客户端上重新生成它们。因此可确保 Unity 在主机迁移期间不会丢失玩家状态数据。

注意：在主机迁移期间，Unity 仅保留客户端可用的数据。如果数据仅在服务器上，则成为新主机的客户端无法使用该数据。主机上的数据仅在主机迁移后才可用（如果位于 SyncVar 或 SyncList 中）。

当客户端成为新主机时，Unity 会为所有联网游戏对象调用回调函数 [OnStartServer](../ScriptReference/Networking.NetworkBehaviour.OnStartServer.html)。在新主机上，Network Migration Manager 使用函数 [BecomeNewHost](../ScriptReference/Networking.NetworkMigrationManager.BecomeNewHost.html) 基于当前 [ClientScene](../ScriptReference/Networking.ClientScene.html) 中的状态构造联网的服务器场景。

在启用主机迁移的游戏中，由服务器上的 [connectionId](../ScriptReference/Networking.NetworkSystem.PeerInfoMessage-connectionId.html) 来标识对等方。当客户端重新连接到游戏的新主机时，Unity 将此 connectionId 传递给新主机，这样新主机可以将该客户端与先前连接到旧主机的客户端匹配。此 ID 在 `ClientScene` 上显示为 [reconnectId](../ScriptReference/Networking.ClientScene-reconnectId.html)。

## 非玩家游戏对象

具有客户端授权的非玩家游戏对象也由主机迁移进行处理。就像禁用和重新启用玩家游戏对象一样，Unity 也会禁用和重新启用客户端拥有的非玩家游戏对象。

## 对等方标识

在主机断开连接之前，所有对等方都连接到主机。这些对等方在主机上都有唯一的 connectionId，在主机迁移的上下文中称为 [oldConnectionId](../ScriptReference/Networking.NetworkSystem.ReconnectMessage-oldConnectionId.html)。

当 Network Migration Manager 选择新主机并且对等方重新连接到该主机时，这些对等方会提供 `oldConnectionId` 来标识自己是哪个对等方。这样一来，新主机就会将这个重新连接的客户端与相应的玩家游戏对象进行匹配。

旧主机不存在与旧主机的连接，它本身就是旧主机，因此使用等于零的特殊 `oldConnectionId` 进行重新连接。有一个常量 [ClientScene.ReconnectIdHost](../ScriptReference/Networking.ClientScene.ReconnectIdHost.html) 用于此目的。

使用 Network Migration Manager 的内置用户界面时，Network Migration Manager 会自动设置 `oldConnectionId`。若要进行手动设置，请使用 [NetworkMigrationManager.Reset](../ScriptReference/Networking.NetworkMigrationManager.Reset.html) 或 [ClientScene.SetReconnectId](../ScriptReference/Networking.ClientScene.SetReconnectId.html)。

## 主机迁移流程

* MachineA 托管 Game1（这是启用了主机迁移的游戏）

* MachineB 启动客户端并加入 Game1

    * MachineB 被告知有对等方（MachineA-0 和自身 (MachineB)–1）

* MachineC 启动客户端并加入 Game1

    * MachineC 被告知有对等方（MachineA-0、MachineB-1 和自身 (MachineC)–2）

* MachineA 在 Game 1 上中断连接，因此主机断开连接

* MachineB 与主机断开连接

    * MigrationManager 在客户端上调用 MachineB 回调函数

    * 所有玩家的 MachineB 玩家游戏对象都被禁用

    * MachineB 保持在线场景

* MachineB 使用实用函数来挑选新主机，选取自身作为新主机

    * MachineB 调用 BecomeNewHost()

    * MachineB 开始监听

    * MachineB 自身的玩家游戏对象被重新激活

    * MachineB 的玩家现在已恢复自己原来的全部游戏状态

* MachineC 与主机断开连接

    * MigrationManager 在客户端上调用 MachineC 回调函数

    * 所有玩家的 MachineC 玩家游戏对象都被禁用

    * MachineC 保持在线场景

* MachineC 使用实用函数来挑选新主机，选取 MachineB

    * MachineC 重新连接到新主机

* MachineB 接收来自 MachineC 的连接

    * MachineC 使用 oldConnectionId 发送重新连接消息（而不是 AddPlayer 消息）

    * MigrationManager 在服务器上调用回调函数

    * MachineB 使用 oldConnectionId 查找该玩家的已禁用玩家游戏对象，并使用 ReconnectPlayerForConnection() 重新添加该玩家

    * 为 MachineC 重新生成玩家游戏对象

    * MachineC 的玩家现在已恢复自己原来的全部游戏状态

* MachineA 恢复（旧主机）

    * MachineA 使用实用函数来挑选新主机，选取 MachineB

    * MachineA“重新连接”到 MachineB

* MachineB 接收来自 MachineA 的连接

* MachineA 以等于零的 oldConnectionId 发送重新连接消息

    * MigrationManager 在服务器 (MachineB) 上调用回调函数

    * MachineB 使用 oldConnectionId 查找该玩家的已禁用玩家游戏对象，并使用 ReconnectPlayerForConnection() 重新添加该玩家

    *为 MachineA 重新生成玩家游戏对象

    * MachineA 的玩家现在已恢复自己原来的全部游戏状态

## 回调函数

NetworkHostMigrationManager 上的回调函数：

```

// 在与主机的连接中断后在客户端上调用。控制是否切换场景
protected virtual void OnClientDisconnectedFromHost(
    NetworkConnection conn, 
    out SceneChangeOption sceneChange)

// 在主机中断后在主机上调用。主机必须改变场景
protected virtual void OnServerHostShutdown()

// 当来自旧主机的客户端重新与玩家连接时，在新主机（服务器）上调用
protected virtual void OnServerReconnectPlayer(
    NetworkConnection newConnection, 
    GameObject oldPlayer, 
    int oldConnectionId, 
    short playerControllerId)

// 当来自旧主机的客户端重新与玩家连接时，在新主机（服务器）上调用
protected virtual void OnServerReconnectPlayer(
    NetworkConnection newConnection, 
    GameObject oldPlayer, 
    int oldConnectionId, 
    short playerControllerId, 
    NetworkReader extraMessageReader)

// 当来自旧主机的客户端重新与非玩家游戏对象连接时，在新主机（服务器）上调用
protected virtual void OnServerReconnectObject(
    NetworkConnection newConnection, 
    GameObject oldObject, 
    int oldConnectionId)

// 当更新对等方集合时，同时在主机和客户端上调用
protected virtual void OnPeersUpdated(
    PeerListMessage peers)

// 在与主机的连接中断后，由客户端上的默认 UI 调用的实用函数，用于挑选新的主机。
public virtual bool FindNewHost(
    out NetworkSystem.PeerInfoMessage newHostInfo, 
    out bool youAreNewHost)

// 当非玩家游戏对象的授权改变时调用
protected virtual void OnAuthorityUpdated(
    GameObject go,
    int connectionId,
    bool authorityState)
```

## 约束

为使主机迁移正常工作，需要访问游戏对象的 Network Manager 组件并启用 **Auto Create Player**。当主机断开连接时，仅保存在服务器（主机）上的数据将会丢失。为使游戏能够正确执行主机迁移，必须将重要数据分发给客户端，而不是秘密保存在服务器上。

这种情况适用于直接连接的游戏。需要执行额外的工作才能与 Matchmaker 和 Relay Server 一起工作。

