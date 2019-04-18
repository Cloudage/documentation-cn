# 使用传输层 API

除了[高级网络 API (HLAPI)](UNetUsingHLAPI.html) 之外，Unity 还允许访问称为**传输层**的低级网络 API。传输层允许您构建自己的网络系统，满足在游戏网络方面更具体或更高级的要求。

传输层是在操作系统基于套接字的网络之上运行的一个薄[层](https://en.wikipedia.org/wiki/Multitier_architecture)。传输层可以发送和接收以字节数组表示的消息，并提供许多不同的“[服务质量](../ScriptReference/Networking.QosType.html)”选项来满足不同的需求。该层专注于灵活性和性能，并在 [NetworkTransport](../ScriptReference/Networking.NetworkTransport.html) 类中公开一个 API。

传输层支持网络通信的基本服务。这些基本服务包括：

* 建立连接

* 使用各种“服务质量”进行通信

* 流量控制

* 基本统计信息

* 其他服务，例如通过中继服务器 (Relay Server) 或本地发现进行通信


传输层可以使用两种协议：用于一般通信的 UDP 和用于 WebGL 的 WebSocket。要直接使用传输层，一般工作流程如下：

1.初始化网络传输层

2.配置网络拓扑

3.创建主机

4.开始通信（处理连接和发送/接收消息）

5.使用之后关闭库

请参阅下面的相应部分以了解每个部分的技术细节。每个部分都提供了一个可添加到网络脚本中的代码片段。

### 步骤 1：初始化网络传输层

初始化网络传输层时，可在下面代码示例中演示的默认初始化（没有参数）之间进行选择，或者也可提供其他参数（例如最大数据包大小和线程超时限制）来控制网络层的整体行为。

要使用默认设置来初始化传输层，请调用 [Init()](../ScriptReference/Networking.NetworkTransport.Init.html)：

```

    // 在不使用任何参数的情况下初始化传输层（默认设置）
        NetworkTransport.Init();

    要使用您自己的配置来初始化传输层，只需将配置作为参数添加到 Init，如下所示。

        // 使用自定义设置来初始化传输层的示例
        GlobalConfig gConfig = new GlobalConfig();
        gConfig.MaxPacketSize = 500;
        NetworkTransport.Init(gConfig);
```

仅当具有特殊的网络环境并且熟悉所需的特定设置时，才应使用自定义 `Init` 值。根据经验，如果要开发基于互联网的典型多人游戏，默认的 `Init` 设置就足够了。

### 步骤 2：配置网络拓扑

下一步是配置对等方之间的连接。网络拓扑定义了允许的连接数和要使用的连接配置。如果游戏需要发送不同重要程度的网络消息（例如，偶然声音效果的重要性较低，玩家是否得分的声音效果的重要性较高），您可能需要定义几个通信通道，根据要发送的具体消息类型以及消息在游戏中的相对重要性，为每个通道指定不同的服务质量级别。

```

    ConnectionConfig config = new ConnectionConfig();
        int myReliableChannelId  = config.AddChannel(QosType.Reliable);
        int myUnreliableChannelId = config.AddChannel(QosType.Unreliable);
```

上面的示例定义了具有不同服务质量值的两个通信通道。[QosType.Reliable](../ScriptReference/Networking.QosType.Reliable.html) 传递消息并确保消息传递成功，而 [QosType.Unreliable](../ScriptReference/Networking.QosType.Unreliable.html) 以更快速度发送消息，但不会进行任何检查来确保消息传递成功。

还可以调整 [ConnectionConfig](../ScriptReference/Networking.ConnectionConfig.html) 上的属性来指定每个连接的配置设置。但是，在从一个客户端连接到另一个客户端时，两个连接对等方的设置应该相同，否则连接会因 [CRCMismatch](../ScriptReference/Networking.NetworkError.CRCMismatch.html) 错误而失败。

网络配置的最后一步是拓扑定义。

```

HostTopology topology = new HostTopology(config, 10);
```

此示例将主机拓扑定义为最多允许 10 个连接。这些连接是在步骤 1 中配置的连接。

### 步骤 3：创建主机

执行前两个初步设置步骤之后，现在需要创建一个主机（打开套接字）：

```

int hostId = NetworkTransport.AddHost(topology, 8888);
```

此代码示例在端口 8888 和任何 IP 地址上添加新主机。主机最多支持 10 个连接（在步骤 2 中配置）。这些连接是在步骤 1 中配置的连接。

### 步骤 4：开始通信

要开始通信，必须建立与其他主机的连接。为此，请调用 `Connect()`。这样将在您和远程主机之间建立连接。此过程会收到一个事件以指示连接是否成功。

首先，使用端口 8888 连接到地址为 192.168.1.42 的远程主机。此情况下会返回分配的 `connectionId`。

```

connectionId = NetworkTransport.Connect(hostId, "192.168.1.42", 8888, 0, out error);

```

连接完成后，将收到 ConnectEvent。现在可以开始发送数据了。

```

NetworkTransport.Send(hostId, connectionId, myReliableChannelId, buffer, bufferLength,  out error);
```

完成连接后，请调用 `Disconnect()` 以断开主机连接。

```
NetworkTransport.Disconnect(hostId, connectionId, out error);
```

要检查函数调用是否成功，可将 `out error` 转换为 [NetworkError](../ScriptReference/Networking.NetworkError.html)。[NetworkEror.Ok](../ScriptReference/Networking.NetworkError.Ok.html) 表示未遇到任何错误。

要检查主机状态，可使用两个函数：

要轮询内部事件队列中的事件，可调用

NetworkTransport.Receive(out recHostId, out connectionId, out channelId, recBuffer, bufferSize, out dataSize, out error);

或
NetworkTransport.ReceiveFromHost(recHostId, out connectionId, out channelId, recBuffer, bufferSize, out dataSize, out error);

这两个函数都从队列中返回事件；第一个函数将从任何主机返回事件，为 recHostId 变量分配作为消息来源的主机的 ID，而第二个函数仅从提供的 recHostId 所指定的主机返回事件。

一种从 `Receive` 轮询数据的方式是在 `Update` 函数中调用它：

```

void Update()
{

    int recHostId; 
    int connectionId; 
    int channelId; 
    byte[] recBuffer = new byte[1024]; 
    int bufferSize = 1024;
    int dataSize;
    byte error;
    NetworkEventType recData = NetworkTransport.Receive(out recHostId, out connectionId, out channelId, recBuffer, bufferSize, out dataSize, out error);
    switch (recData)
    {
        case NetworkEventType.Nothing:                     break;
        case NetworkEventType.ConnectEvent:                break;
        case NetworkEventType.DataEvent:                   break;
        case NetworkEventType.DisconnectEvent:            break;

  case NetworkEventType.BroadcastEvent:

       break;
    }
}

```

可收到 5 种类型的事件。

* [NetworkEventType.Nothing](../ScriptReference/Networking.NetworkEventType.Nothing.html)：事件队列没有要报告的内容。

* [NetworkEventType.ConnectEvent](../ScriptReference/Networking.NetworkEventType.ConnectEvent.html)：您收到了一个连接事件。这可能是成功的连接请求，也可能是连接响应。

```

case NetworkEventType.ConnectEvent: 
    if(myConnectionId == connectionId)
        //我的连接请求已被批准
    else
        //其他人向我发送了连接请求
    break;

```

* [NetworkEventType.DataEvent](../ScriptReference/Networking.NetworkEventType.DataEvent.html)：您收到了一个数据事件。当有一些数据准备好接收时，您会收到数据事件。如果 `recBuffer` 大到足以容纳数据，则数据将复制到缓冲区中。如果不够大，该事件包含 [MessageToLong](../ScriptReference/Networking.NetworkError.MessageToLong.html) 网络错误。如果发生这种情况，需要为缓冲区重新分配更大的大小并再次调用 `DataEvent` 函数。

* [NetworkEventType.DisconnectEvent](../ScriptReference/Networking.NetworkEventType.DisconnectEvent.html)：建立的连接已断开连接，或者您的连接请求已失败。请检查错误代码以找出发生这种情况的原因。

```

case NetworkEventType.DisconnectEvent: 
    if(myConnectionId == connectionId)
        //由于某种原因无法连接，请参阅错误
    else
        //其中一个建立的连接已断开连接
    break;
```

* [NetworkEventType.BroadcastEvent](../ScriptReference/Networking.NetworkEventType.BroadcastEvent.html)：表示您已收到广播事件，现在可以调用 [GetBroadcastConnectionInfo](../ScriptReference/Networking.NetworkTransport.GetBroadcastConnectionInfo.html) 和 [GetBroadcastConnectionMessage](../ScriptReference/30_search.html?q=GetBroadcastConnectionMessage.html) 来检索更多信息。

### WebGL 支持

可在 WebGL 上使用 WebSocket，但 Web 客户端只能连接到主机，本身不能是主机。这意味着主机必须是单机玩家（仅限 Win、Mac 或 Linux）。对于客户端配置，上述所有步骤（包括拓扑和配置）都是相同的。在服务器上，调用以下函数：

```

NetworkTransport.AddWebsocketHost(topology, port, ip);

```

上面的 IP 地址应该是需要监听的具体地址，或者如果希望主机监听所有网络接口，则可以传递 null 作为 IP 地址。

服务器一次只能支持一个 WebSocket 主机，但可以同时处理其他通用主机：

````

NetworkTransport.AddWebsocketHost(topology, 8887, null);
NetworkTransport.AddHost(topology, 8888);

````
