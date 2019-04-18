# 自定义玩家生成

Network Manager 提供了内置的简单玩家生成功能，但您可能希望自定义玩家生成过程，例如为每个生成的新玩家分配颜色。

为此，需要使用自定义的脚本重写 Network Manager 的默认行为。


Network Manager 添加玩家时，还会从玩家预制件中实例化游戏对象并将游戏对象与连接相关联。为此，Network Manager 会调用 [NetworkServer.AddPlayerForConnection](../ScriptReference/Networking.NetworkServer.AddPlayerForConnection.html)。您可以通过重写 [NetworkManager.OnServerAddPlayer](../ScriptReference/Networking.NetworkManager.OnServerAddPlayer.html) 来修改此行为。`OnServerAddPlayer` 的默认实现将从玩家预制件实例化一个新的玩家实例，并调用 [NetworkServer.AddPlayerForConnection](../ScriptReference/Networking.NetworkServer.AddPlayerForConnection.html) 来生成新的玩家实例。`OnServerAddPlayer` 的自定义实现也必须调用 `NetworkServer.AddPlayerForConnection`，但也可在该方法中任意执行所需的任何其他初始化。

下面的示例自定义了玩家的颜色。首先，将颜色脚本添加到玩家预制件：

```
using UnityEngine;
using UnityEngine.Networking;
class Player : NetworkBehaviour
{
	[SyncVar]
	public Color color;
} 
```

然后，创建 [NetworkManager](../ScriptReference/Networking.NetworkManager.html) 来处理生成。

```
using UnityEngine;
using UnityEngine.Networking;
public class MyNetworkManager : NetworkManager
{
	public override void OnServerAddPlayer(NetworkConnection conn, short playerControllerId)
	{
		GameObject player = (GameObject)Instantiate(playerPrefab, Vector3.zero, Quaternion.identity);
		player.GetComponent<Player>().color = Color.red;
		NetworkServer.AddPlayerForConnection(conn, player, playerControllerId);
	}
} 
```

不必从 `OnServerAddPlayer` 中调用 `NetworkServer.AddPlayerForConnection` 函数。只要传入正确的连接对象和 `playerControllerId`，就可以在 `OnServerAddPlayer` 返回后调用该函数。因此可在两者之间执行异步步骤，例如从远程数据源加载玩家数据。

虽然在大多数多人游戏中，通常需要为每个客户端分配一个玩家，但 [HLAPI](UNetUsingHLAPI.html) 将玩家和客户端视为单独的概念。这是因为，在某些情况下（例如，如果有多个控制器连接到游戏主机系统），对于单个连接可能需要多个玩家游戏对象。当一个连接上有多个玩家时，应使用 `playerControllerId` 属性来区分它们。这是一个仅用于连接作用的标识符，因此可以映射到与该客户端上的玩家相关联的控制器 ID。

系统会自动生成传递给服务器上的 `NetworkServer.AddPlayerForConnection` 的玩家游戏对象，因此您无需为玩家调用 [NetworkServer.Spawn](../ScriptReference/Networking.NetworkServer.Spawn.html)。一旦玩家准备就绪，场景中处于活动状态的联网游戏对象（即具有相关 [NetworkIdentity](UNetSpawning.html) 的游戏对象）就会在玩家的客户端上生成。游戏中的所有联网游戏对象都是在该客户端上以最新状态创建的，因此它们与游戏的其他参与者同步。

无需在 `NetworkManager` 上使用 [playerPrefab](../ScriptReference/Networking.NetworkManager-playerPrefab.html) 创建玩家游戏对象。可使用不同的方法创建不同的玩家。

## 就绪状态

除了玩家之外，客户端连接也具有“就绪”状态。主机向已就绪的客户端发送有关生成的游戏对象和状态同步更新的信息；未就绪的客户端不会收到这些更新。客户端最初连接到服务器时尚未就绪。在处于这种非就绪状态时，客户端可以执行不需要与服务器上的游戏状态进行实时交互的操作，例如加载场景、允许玩家选择头像或填写登录框。一旦客户端完成了所有游戏前工作，并且所有资源都已加载后，客户端便可调用 [ClientScene.Ready](../ScriptReference/Networking.ClientScene-ready.html) 以进入“就绪”状态。上面的简单示例演示了就绪状态的实现；因为使用 `NetworkServer.AddPlayerForConnection` 添加玩家也会将客户端置于就绪状态（如果尚未处于该状态）。

客户端可尚未就绪的情况下发送和接收网络消息，这也意味着它们可以在没有活动玩家游戏对象的情况下执行此操作。因此，处于菜单或选择屏幕的客户端可以连接到游戏并与之交互，即使它们没有玩家游戏对象也是如此。如需了解在不使用命令和 RPC 调用的情况下发送消息的更多详细信息，请参阅[网络消息](UNetMessages.html)文档。

## 切换玩家

要替换用于连接的玩家游戏对象，请使用 [NetworkServer.ReplacePlayerForConnection](../ScriptReference/Networking.NetworkServer.ReplacePlayerForConnection.html)。此功能可用于限制玩家在特定时间（例如在游戏前的大厅屏幕中）可发出的命令。此函数采用与 `AddPlayerForConnection` 相同的参数，但允许该连接的已有玩家。不必销毁旧玩家游戏对象。当大厅中的所有玩家都准备就绪时，[NetworkLobbyManager](../ScriptReference/Networking.NetworkLobbyManager.html) 使用此技术从 [NetworkLobbyPlayer](../ScriptReference/Networking.NetworkLobbyPlayer.html) 游戏对象切换到游戏玩家游戏对象。

还可以使用 `ReplacePlayerForConnection` 在游戏对象被销毁后重新生成一个玩家。在某些情况下，最好直接禁用游戏对象并在重新生成时重置其游戏属性。以下代码示例演示了如何使用新的游戏对象实际替换已销毁的游戏对象：

```
class GameManager
{
    public void PlayerWasKilled(Player player)
    {
        var conn = player.connectionToClient;
        var newPlayer = Instantiate<GameObject>(playerPrefab);
        Destroy(player.gameObject);
        NetworkServer.ReplacePlayerForConnection(conn, newPlayer, 0);
    }
}
```

如果用于连接的玩家游戏对象被销毁，则该客户端无法执行命令。但是，客户端仍然可以发送网络消息。

要使用 `ReplacePlayerForConnection`，必须拥有 [NetworkConnection](../ScriptReference/Networking.NetworkConnection.html) 游戏对象以便玩家的客户端建立游戏对象与客户端之间的关系。这通常是 [NetworkBehaviour](../ScriptReference/Networking.NetworkBehaviour.html) 类上的 [connectionToClient](../ScriptReference/Networking.NetworkBehaviour-connectionToClient.html) 属性，但如果旧玩家已经被销毁，那么这可能不是现成的。

要查找连接，可使用一些列表。如果使用 `NetworkLobbyManager`，则可使用 [lobbySlots](../ScriptReference/Networking.NetworkLobbyManager-lobbySlots.html) 中的大厅玩家。[NetworkServer](../ScriptReference/Networking.NetworkServer.html) 还具有 [connections](../ScriptReference/Networking.NetworkServer-connections.html) 和 [localConnections](../ScriptReference/Networking.NetworkServer-localConnections.html) 的列表。
