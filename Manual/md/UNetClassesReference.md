#Multiplayer 类参考

可创建从以下类继承的脚本来自定义 Unity 网络的工作方式。

* [NetworkBehaviour](class-NetworkBehaviour.html) 类处理具有 [Network Identity](class-NetworkIdentity.html) 组件的游戏对象。这些脚本可以执行[高级 API](UNetUsingHLAPI.html) 函数，例如 [Command、ClientRPC](UNetActions.html)、[SyncEvent](../ScriptReference/Networking.SyncEventAttribute.html) 和 [SyncVar](UNetStateSync.html)。
* [NetworkClient](class-NetworkClient.html) 类负责管理从客户端到服务器的网络连接，还可以在客户端与服务器之间发送和接收消息。
* [NetworkConnection](class-NetworkConnection.html) 负责封装网络连接。(NetworkClient)[class-NetworkClient] 对象具有一个 `NetworkConnection`，而 [NetworkServer](class-NetworkServer.html) 具有多个连接：与每个客户端有一个连接。NetworkConnection 能够作为网络消息来发送字节数组或序列化对象。
* [NetworkServer](class-NetworkServer.html) 负责管理来自多个客户端的连接，并提供游戏相关功能，比如生成、本地客户端和玩家管理器。
* [NetworkServerSimple](class-NetworkServerSimple.html) 是一个不含任何游戏相关功能的基本服务器类。NetworkServer 类负责处理游戏类型内容（比如生成、本地客户端和玩家管理器）而且有一个静态接口，而 NetworkServerSimple 类是一个纯网络服务器，无任何游戏相关功能。NetworkServerSimple 也没有任何静态接口或单例，所以在同一时间内，在一个进程中可存在多个实例。
