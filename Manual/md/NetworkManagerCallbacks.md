# Network Manager 回调

在正常的多人游戏运行过程中，可能会发生很多事件，比如主机启动、玩家加入或玩家离开。每个可能的此类事件都有一个关联的**回调**；您可以用自己的代码来实现此回调以便在发生事件时采取操作。

要对 Network Manager 执行此操作，必须自行创建继承自 [NetworkManager](../ScriptReference/Networking.NetworkManager.html) 的脚本。然后，可以将 NetworkManager 上的虚拟方法[重写](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/override)为发生指定事件时应发生的操作的自定义实现。

本页面列出了可在 Network Manager 上实现的所有虚拟方法（回调）以及这些虚拟方法发生的时间。根据游戏是在 LAN 模式下运行还是在互联网（配对）模式下运行，具体发生的回调以及发生的顺序略有不同，因此下面单独列出了每个模式的回调。



## LAN 回调

以下是当游戏在本地连接 (LAN) 上运行时发生的回调。游戏可采用以下三种模式之一运行：**主机**、**客户端**或**仅服务器**。下面列出了每种模式的回调：

### 主机模式下的 LAN 回调：

**当主机启动时：**

* 调用 `Start()` 函数

* `OnStartHost`

* `OnStartServer`

* `OnServerConnect`

* `OnStartClient`

* `OnClientConnect`

* `OnServerSceneChanged`

* `OnServerReady`

* `OnServerAddPlayer`

* `OnClientSceneChanged`

**当客户端连接时：**

* `OnServerConnect`

* `OnServerReady`

* `OnServerAddPlayer`

**当客户端断开连接时：**

* `OnServerDisconnect`

**当主机停止时：**

* `OnStopHost`

* `OnStopServer`

* `OnStopClient`

### 客户端模式下的 LAN 回调

**当客户端启动时：**

* 调用 `Start()` 函数

* `OnStartClient`

* `OnClientConnect`

* `OnClientSceneChanged`

**当客户端停止时：**

* `OnStopClient`

* `OnClientDisconnect`

### 服务器模式下的 LAN 回调

**当服务器启动时：**

* 调用 `Start()` 函数

* `OnStartServer`

* `OnServerSceneChanged`

**当客户端连接时：**

* `OnServerConnect`

* `OnServerReady`

* `OnServerAddPlayer`

**当客户端断开连接时：**

* `OnServerDisconnect`

**当服务器停止时：**

* `OnStopServer`

## MatchMaker 连接回调

以下是当游戏在互联网模式下运行时（即，使用 MatchMaker 服务来查找和连接到其他玩家时）发生的回调。在此模式下，游戏可采用以下两种模式之一运行：**主机**或**客户端**。下面列出了每种模式的回调：

### 主机模式下的 MatchMaker 回调

**当主机启动时：**

* 调用 `Start()` 函数

* `OnStartHost`

* `OnStartServer`

* `OnServerConnect`

* `OnStartClient`

* `OnMatchCreate`

* `OnClientConnect`

* `OnServerSceneChanged`

* `OnServerReady`

* `OnServerAddPlayer`

* `OnClientSceneChanged`


**当客户端连接时：**

* `OnServerConnect`

* `OnServerReady`

* `OnServerAddPlayer`


**当客户端断开连接时：**

* `OnServerDisconnect`

### MatchMaker callbacks in client mode

**接收联机游戏实例的列表时：**

* 调用 `Start()` 函数

* `OnMatchList`

**当加入比赛时：**

* `OnStartClient`

* `OnMatchJoined`

* `OnClientConnect`

* `OnClientSceneChanged`

**当主机停止时：**

* `OnStopClient`

* `OnClientDisconnect`
