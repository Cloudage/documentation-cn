# Network Discovery

[Network Discovery 组件](class-NetworkDiscovery.html)允许 Unity 多人游戏在局域网 (LAN) 上相互查找。这意味着玩家无需找到主机的 IP 地址即可连接到 LAN 上的游戏。Network Discovery 在互联网上无效，只对局域网有效。对于基于互联网的游戏，请参阅 [MatchMaker](UnityMultiplayerSettingUp.html) 服务。

Network Discovery 组件可以广播其存在性，监听来自其他 Network Discovery 组件的广播，选择性使用 [Network Manager](class-NetworkManagerUNet.html) 加入匹配的游戏。Network Discovery 组件使用网络传输层的 UDP 广播功能。

要使用本地网络发现，请在场景中创建一个空游戏对象，并为其添加 Network Discovery 组件。

![NetworkDiscovery 组件](../uploads/Main/UNetDiscovery.png)

与 [Network Manager HUD](class-NetworkManagerHUD.html) 一样，该组件在 Game 视图中显示一个具有控制功能的默认 GUI，用于临时开发工作，并假设您会在完成游戏之前创建自己的替代界面。请注意，还需要在场景中添加一个 Network Manager 组件才能通过 Network Discovery GUI 加入游戏。当游戏开始时，单击 Network Discovery GUI 中的 **Initialize Broadcast** 按钮（在 Game 视图中）即可发送广播并开始在本地网络上发现其他游戏。

Network Discovery 组件可以在服务器模式（通过单击 GUI 中的“Start Broadcasting”按钮激活）或客户端模式（通过单击 GUI 中的“Listen for Broadcast”按钮激活）下运行。

在**服务器模式**下，Network Discovery 组件通过网络在 Inspector 中指定的端口上发送广播消息。这些消息包含游戏的**广播密钥 (Broadcast ****Key)** 和**广播版本 (Broadcast Version)**。可将这些属性设置为所需的任何值，目的是识别游戏的具体版本以避免冲突（例如游戏尝试加入不同类型的游戏）。您在发布不允许连接到游戏旧版本的新版本时，应更改**广播密钥 (Broadcast Key)** 的值。如果在本地计算机上托管游戏，该组件应以服务器模式运行。如果没有默认 GUI，则需要调用 [StartAsServer()](../ScriptReference/Networking.NetworkDiscovery.StartAsServer.html) 函数来使该组件在服务器模式下运行。

在**客户端模式**下，Network Discovery 组件在指定端口上监听广播消息。收到消息时，如果消息中的**广播密钥 (Broadcast Key)**与 Network Discovery 组件中的**广播密钥 (Broadcast Key)** 匹配，则表示可以在本地网络上加入某个游戏。如果没有默认 GUI，则需要调用 [StartAsClient()](../ScriptReference/Networking.NetworkDiscovery.StartAsClient.html) 函数来使该组件在客户端模式下运行。

使用默认 GUI 并在客户端模式下监听广播时，如果在本地网络上发现游戏，则会出现一个按钮，允许该客户端的用户加入游戏。该按钮显示为“Game at:”后跟主机的 IP 地址。

Network Discovery 组件上有一个可以实现的虚拟函数用于在收到广播消息时获得通知。

```
public class MyNetworkDiscovery: NetworkDiscovery {
	public override void OnReceivedBroadcast(string fromAddress, string data)
	{
		Debug.Log("Received broadcast from: " + fromAddress+ " with the data: " + data);
	}
}
```

有关更多信息，请参阅关于 [NetworkDiscovery](../ScriptReference/Networking.NetworkDiscovery.html) 的脚本 API 参考文档。请注意，不能同时在同一进程中运行 Network Discovery 服务器和客户端。

