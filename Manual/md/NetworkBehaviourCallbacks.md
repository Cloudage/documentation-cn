# NetworkBehaviour 回调

类似于 [Network Manager 回调](https://docs.unity3d.com/Manual/NetworkManagerCallbacks.html)，在正常的多人游戏过程中，可能会发生很多与网络行为相关的事件。这些事件包括主机启动、玩家加入或玩家离开等事件。每个可能的此类事件都有一个关联的**回调**；您可以用自己的代码来实现此回调以便在发生事件时采取操作。

创建一个**继承**自 [NetworkBehaviour](../ScriptReference/Networking.NetworkBehaviour.html) 的脚本时，可以编写发生这些事件时应采取的操作的自定义实现。为此，需要使用发生这些事件时应采取的操作的自定义实现来[重写](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/override) `NetworkBehaviour` 类中的虚拟方法。

本页面列出了可在 Network Behaviour 上实现的所有虚拟方法（回调）以及这些虚拟方法发生的时间。游戏可进入以下三种模式之一：**主机**、**客户端**或**仅服务器**。下面列出了每种模式的回调：

### 服务器模式下的回调

**当客户端连接时：**

* `OnStartServer`

* `OnRebuildObservers`

* 调用 `Start()` 函数

### 客户端模式下的回调

**当客户端连接时：**

* `OnStartClient`

* `OnStartLocalPlayer`

* `OnStartAuthority`

* 调用 `Start()` 函数

### 主机模式下的回调

仅当客户端连接时才会在**玩家游戏对象**上调用以下回调：

* `OnStartServer`

* `OnStartClient`

* `OnRebuildObservers`

* `OnStartAuthority`

* `OnStartLocalPlayer`

* 调用 `Start()` 函数

* `OnSetLocalVisibility`

**在所有其余客户端上，当客户端断开连接时：**

* `OnNetworkDestroy`
