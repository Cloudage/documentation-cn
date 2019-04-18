Network Lobby Manager
=====================

NetworkLobbyManager 是一种专用类型的 NetworkManager，可在进入游戏的主游戏场景之前提供多人游戏大厅。此组件可用于设置网络的以下属性：

* 玩家最大限制
* 所有玩家准备好后自动开始
* 用于阻止玩家加入正在进行的游戏的选项
* 支持“Couch Multiplayer”（即每个客户端有多个玩家）
* 可自定义玩家在大厅选择选项的方式

NetworkLobbyManager 有两种类型的玩家对象：

LobbyPlayer 对象

* 每个玩家一个
* 客户端连接时或添加玩家时创建
* 在客户端断开连接之前一直存在
* 保留就绪标志和配置数据
* 处理大厅中的命令
* 应使用 NetworkLobbyPlayer 组件

GamePlayer 对象

* 每个玩家一个
* 启动游戏场景时创建
* 重新进入大厅时被销毁
* 处理游戏中的命令


属性
----------

|**_属性：_** |**_功能：_** |
|:---|:---|
|__showLobbyGUI__ |显示大厅的开发者 OnGUI 控件。 |
|__maxPlayers__ |大厅中允许的最大玩家数量。 |
|__maxPlayersPerConnection__ |允许为每个客户端连接添加的最大玩家数。 |
|__lobbyPlayerPrefab__ |当玩家进入大厅时为玩家创建的预制件。 |
|__gamePlayerPrefab__ |当游戏启动时为玩家创建的预制件。 |
|__lobbyScene__ |要用于大厅的场景。 |
|__playScene__ |要用于主要游戏的场景。 |



详细信息
-------

* lobbyPlayerPrefab 字段应该由一个包含 NetworkLobbyPlayer 组件的对象填充。
* Lobby Manager 有一个 GUI。请参阅多人游戏大厅资源包。


#Network Lobby Manager

Network Lobby Manager 是一种专用类型的 NetworkManager，可在进入游戏的主游戏场景之前提供易于使用的多人游戏大厅。

Network Lobby Manager 具有许多内置功能，这些功能对于多人游戏来说很常见。例如，它支持设置最大玩家数量限制，在所有玩家准备就绪时自动启动游戏，以及防止玩家加入正在进行的游戏的选项。Network Lobby Manager 还支持“Couch Multiplayer”，让多个玩家使用同一个客户端一起玩游戏。

![Network Lobby Manager 组件](../uploads/Main/NetworkLobbyManager.png)

|**_属性：_** |**_功能：_** |
|:---|:---|
|**Show Lobby GUI**|启用此属性可显示大厅的开发者 GUI 控件。此属性仅用于方便开发者。您应该为玩家创建您自己的 UI 以便在完成的游戏中使用。|  
|**Max Players**|大厅中允许的最大玩家数量。|  
|**Max Players Per Connection**|允许为每个客户端连接添加的最大玩家数。|  
|**Min Players**|大厅需要的玩家的最低数量。|  
|**Lobby Player Prefab**|当玩家进入大厅时为玩家创建的预制件。|  
|**Game Player Prefab**|当游戏启动时为玩家创建的预制件。|  
|**Lobby Scene**|要用于大厅的场景。|  
|**Play Scene**|要用于主要游戏的场景。|
|**Network Info**|You can expand this section of the inspector to access network-related settings, listed below|
|**Use Web Sockets**|When running as a host, enable this setting to make the host listen for WebSocket connections instead of normal transport layer connections, so that WebGL clients can connect to it (if you build your game for the WebGL platform). These WebGL instances of your game cannot act as a host (in either peer-hosted or server-only mode). Therefore, for WebGL instances of your multiplayer game to be able to find each other and play together, you must host a server-only instance of your game running in LAN mode, with a publicly reachable IP address, and it must have this option enabled. This checkbox is unticked by default.|
|**Network Address**|The network address currently in use. For clients, this is the address of the server that is connected to. For servers, this is the local address. This is set to ‘localhost’ by default.|
|**Network Port**|The network port currently in use. For clients, this is the port of the server connected to. For servers, this is the listen port. This is set to 7777 by default.|
|**Server Bind To IP**|Allows you to tell the server whether to bind to a specific IP address. If this checkbox is not ticked, then there is no specific IP address bound to (IP_ANY). This checkbox is unticked by default. Use this if your server has multiple network addresses (eg, internal LAN, external internet, VPN) and you want to specific the IP address to serve your game on.|
|**Server Bind Address**|This field is only visible when the Server Bind To IP checkbox is ticked. Use this to enter the specific IP address that the server should bind to.|
|**Script CRC Check**|When this is enabled, Unity checks that the clients and the server are using matching scripts. This is useful to make sure outdated versions of your client are not connecting to the latest (updated) version of your server. This checkbox is ticked by default. It does this by performing a (CRC check)[https://en.wikipedia.org/wiki/Cyclic_redundancy_check] between the server and client that ensures the NetworkBehaviour scripts match. This may not be appropriate in some cases, such as when you are intentionally using different Unity projects for the client and server. In most other cases however, you should leave it enabled.|
|**Max Delay**|The maximum time in seconds to delay buffered messages. The default of 0.01 seconds means packets are delayed at most by 10 milliseconds. Setting this to zero disables HLAPI connection buffering. This is set to 0.01 by default.|
|**Max Buffered Packets**|The maximum number of packets that a [NetworkConnection](../ScriptReference/Networking.NetworkConnection.html) can buffer for each channel. This corresponds to the [ChannelOption.MaxPendingBuffers](../ScriptReference/Networking.ChannelOption.MaxPendingBuffers.html) channel option. This is set to 16 by default.|
|**Packet Fragmentation**|This allows the `NetworkConnection` instances to fragment packets that are larger than `maxPacketSize`, to up a maximum of 64K. This can cause delays in sending large packets. This checkbox is ticked by default. |
|**MatchMaker Host URI**|The host address for the MatchMaker server. By default this points to the global Unity Multiplayer Service at mm.unet.unity3d.com, and usually you should not need to change this. Unity automatically groups players of your game into regional servers around the world, which ensures fast multiplayer response times between players in the same region. This means, for example, that players from Europe, the US, and Asia generally end up playing with other players from their same global region. You can override this value to explicitly control which regional server your game connects to. You might want to do this via scripting if you want to give your players the option of joining a server outside of their global region. For example, if “Player A” (in the US) wanted to connect to a match created via matchmaker by “Player B” (in Europe), they would need to be able to set their desired global region in your game. Therefore you would need to write a UI feature which allows them to select this. See API reference documentation on [NetworkMatch.baseUri](../ScriptReference/Networking.Match.NetworkMatch-baseUri.html) for more information, and for the regional server URIs.|
|**MatchMaker Port**|The host port for the Matchmaker server.  By default this points to port 443, and usually you should not need to change this.|
|**Match Name**|Define the name of the current match. This is set to "default" by default.|
|**Maximum Match Size**|Define the maximum number of players in the current match. This is set to 4 by default.|
|**SpawnInfo**|You can expand this section of the inspector to access spawn-related settings, listed below|
|**Player Prefab**|Define the default prefab Unity should use to create player GameObjects on the server. Unity creates Player GameObjects in the default handler for [AddPlayer](../ScriptReference/Networking.MsgType.AddPlayer.html) on the server. Implement (OnServerAddPlayer)[https://docs.unity3d.com/ScriptReference/Networking.NetworkManager.OnServerAddPlayer.html] to override this behaviour.|
|**Auto Create Player**|Tick this checkbox if you want Unity to automatically create player GameObjects on connect, and when the Scene changes. This checkbox is ticked by default. Note that if you are using the MigrationManager and you do not enable Auto Create Player, you need to call [ClientScene.SendReconnectMessage](../ScriptReference/Networking.ClientScene.SendReconnectMessage.html) when your client reconnects.|
|**Player Spawn Method**|Define how Unity should decide where to spawn new player GameObjects. This is set to Random by default.|
|&nbsp;&nbsp;&nbsp;&nbsp;Random|Choose Random to spawn players at randomly chosen startPositions.|
|&nbsp;&nbsp;&nbsp;&nbsp;Round Robin|Choose Round Robin to cycle through startPositions in a set list.|
|**Registered Spawnable Prefabs**|Use this list to add prefabs that you want the Network Manager to be aware of, so that it can spawn them. You can also add and remove them via scripting.|
|**Advanced Configuration**|Tick this checkbox to reveal advanced configuration options in the Network Manager Inspector window.|
|&nbsp;&nbsp;&nbsp;&nbsp;Max Connections|Define the maximum number of concurrent network connections to support. This is set to 4 by default.|
|&nbsp;&nbsp;&nbsp;&nbsp;Qos Channels|A list containing the different communication channels the current Network Manager has, and the Quality Of Service (QoS) setting for each channel. Use this list to add or remove channels, and adjust their QoS setting. You can also configure the channels via scripting. For the descriptions of each QoS option, see [QosType](../ScriptReference/Networking.QosType.html).|
|&nbsp;&nbsp;&nbsp;&nbsp;Timeouts||
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Min Update Timeout|Set the minimum time (in milliseconds) the network thread waits between sending network messages. The network thread doesn’t send multiplayer network messages immediately. Instead, it check each connection periodically at a fixed rate to see if it has something to send. This is set to 10ms by default. See API reference documentation on [MinUpdateTimeout](../ScriptReference/Networking.ConnectionConfig.MinUpdateTimeout.html) for more information.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Connect Timeout|Define the amount of time (in milliseconds) Unity should wait while trying to connect before attempting the connection again. This is set to 2000ms by default. See API reference documentation on [ConnectTimeout](../ScriptReference/Networking.ConnectionConfig.ConnectTimeout.html) for more information.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Disconnect Timeout|The amount of time (in milliseconds) before Unity considers a connection to be disconnected. This is set to 2000ms by default. See API reference documentation on [DisconnectTimeout](../ScriptReference/Networking.ConnectionConfig.DisconnectTimeout.html) for more information.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ping Timeout|The amount of time (in milliseconds) between sending pings (also known as “keep-alive” packets). The ping timeout duration should be approximately one-third to one-quarter of the Disconnect Timeout duration, so that Unity doesn’t assume that clients are disconnected until the server has failed to receive at least three pings from the client. This is set to 500ms by default. See API reference documentation on [ConnectionConfig.PingTimeout](../ScriptReference/Networking.ConnectionConfig.PingTimeout.html) for more information.|
|&nbsp;&nbsp;&nbsp;&nbsp;Global Config|These settings relate to the Reactor. The Reactor is the part of the multiplayer system which receives network packets from the underlying operating system, and passes them into the multiplayer system for processing.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Thread Awake Timeout|The timeout duration in milliseconds, used by the Reactor. How the Reactor uses this value depends on which Reactor Model you select (see below). This is set to 1ms by default.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reactor Model|Choose which type of reactor to use. The reactor model defines how Unity reads incoming packets. For most games and applications, the default Select reactor is appropriate. If you want to trade a small delay in the processing of network messages for lower CPU usage and improved battery life, use the Fix Rate reactor.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Select Reactor|This model uses the `select()` API which means that the network thread “awakens” (becomes active) as soon as a packet is available. Using this method means your game gets the data as fast as possible. This is the default Reactor Model setting.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fix Rate Reactor|This model lets the network thread sleep manually for a given amount of time (defined by the value in Thread Awake Timeout) before checking whether there are incoming packets waiting to be processed.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reactor Max Recv Messages|Set the maximum number of messages stored in the receive queue. This is set to 1024 messages by default.|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reactor Max Sent Messages|Set the maximum number of messages stored in the send queue. This is set to 1024 messages by default.|
|**Use Network Simulator**|Tick this checkbox to enable the usage of the network simulator. The network simulator introduces simulated latency and packet loss based on the following settings:|
|&nbsp;&nbsp;&nbsp;&nbsp;Simulated Average Latency|The amount of delay in milliseconds to simulate.|
|&nbsp;&nbsp;&nbsp;&nbsp;Simulated Packet Loss|The amount of packet loss to simulate in percent.|
 

