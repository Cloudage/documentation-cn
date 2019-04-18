# NetworkClient

`NetworkClient` 是一个[高级 API](UNetUsingHLAPI.html) 类，可用于管理从客户端到服务器的网络连接，还可以在客户端与服务器之间发送和接收消息。`NetworkClient` 类还有助于管理生成的网络游戏对象，以及路由 [RPC](UNetActions.html) 消息和网络事件。

有关更多信息，请参阅 [NetworkClient](../ScriptReference/Networking.NetworkClient.html) 脚本参考。

## 属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|**serverIP**|此客户端连接到的服务器的 IP 地址。|
|**serverPort**|此客户端连接到的服务器的端口。|
|**connection**|此 `NetworkClient` 实例正在使用的 NetworkConnection 游戏对象。|
|**handlers**|已注册消息处理程序函数的集合。|
|**numChannels**|已配置 NetworkTransport QoS 通道的数量。|
|**isConnected**|如果客户端已连接到服务器，则为 true。|
|**allClients**|激活状态 NetworkClient（静态）的列表。|
|**active**|如果任何 NetworkClient 处于激活状态（静态），则为 true。|
