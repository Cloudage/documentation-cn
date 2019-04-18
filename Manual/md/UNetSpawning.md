# 生成游戏对象

在 Unity 中，通常使用 `Instantiate()` 来“生成”（即创建）新的游戏对象。但是，在多玩家高级 API 中，“生成”一词具有更具体的含义。在 HLAPI 的服务器授权模型中，在服务器上“生成”游戏对象意味着游戏对象是在连接到服务器的客户端上创建的，并由生成系统进行管理。

使用此系统生成游戏对象后，只要游戏对象在服务器上发生更改，就会向客户端发送状态更新。当 Unity 销毁服务器上的游戏对象时，也会在客户端上销毁此游戏对象。服务器会管理生成的游戏对象以及所有其他联网游戏对象，因此如果其他客户端稍后加入游戏，服务器可以在该客户端上生成游戏对象。这些生成的游戏对象具有一个名为“netId”的唯一网络实例 ID，对于每个游戏对象，此 ID 在服务器和客户端上都是相同的。唯一网络实例 ID 用于通过网络将设置的消息路由到游戏对象，并用于识别游戏对象。

当服务器使用 [Network Identity](class-NetworkIdentity.html) **组件**生成游戏对象时，在客户端上生成的游戏对象具有相同的“状态”。这意味着，该游戏对象与服务器上的游戏对象相同；具有相同的变换、运动状态和（如果使用 [NetworkTransform](../ScriptReference/Networking.NetworkTransform.html) 和 [SyncVar](UNetStateSync.html)）同步变量。因此，当 Unity 创建客户端游戏对象时，这些游戏对象始终是最新的。这样可避免各种问题，比如游戏对象在错误的初始位置生成，然后在状态更新到达时重新出现在正确位置。

Network Manager 只能从已注册的预制件生成和同步游戏对象，因此必须向 Network Manager 注册希望能够在游戏中生成的特定游戏对象预制件。Network Manager 仅接受附加了 Network Identity 组件的游戏对象预制件，因此在尝试向 Network Manager 注册预制件之前，必须确保将 Network Identity 组件添加到该预制件。

要在 Editor 中向 Network Manager 注册预制件，请选择 Network Manager 游戏对象，然后在 Inspector 中，导航到 Network Manager 组件。单击 **Spawn Info** 旁边的三角形以打开设置，然后在 **Registered Spawnable Prefabs** 下单击加号 (**+**) 按钮。将预制件拖放到空字段中即可将预制件分配给列表。

![展开了 _Spawn Info_* 的 Network Manager Inspector，显示 _Registered Spawnable Prefabs_ 下面有三个已分配的预制件](../uploads/Main/UNetManagerSpawnInfo.png)

## 在不使用 Network Manager 的情况下生成

更高级的用户可能希望注册预制件并在不使用 NetworkManager 组件的情况下生成游戏对象。

要在不使用 Network Manager 的情况下生成游戏对象，可通过脚本自行处理预制件注册。使用 [ClientScene.RegisterPrefab](../ScriptReference/Networking.ClientScene.RegisterPrefab.html) 方法可将预制件注册到 Network Manager。

### 示例：MyNetworkManager

```

using UnityEngine;
using UnityEngine.Networking;

public class MyNetworkManager : MonoBehaviour 
{
    public GameObject treePrefab;
    NetworkClient myClient;

    // 创建客户端并连接到服务器端口
    public void ClientConnect() {
        ClientScene.RegisterPrefab(treePrefab);
        myClient = new NetworkClient();
        myClient.RegisterHandler(MsgType.Connect, OnClientConnect);
        myClient.Connect("127.0.0.1", 4444);
    }

    void OnClientConnect(NetworkMessage msg) {
        Debug.Log("Connected to server: " + msg.conn);
    }
}
```

在此示例中，首先创建一个空游戏对象作为 Network Manager，然后创建 `MyNetworkManager` 脚本（上面的脚本）并将其附加到该游戏对象。创建一个已附加 Network Identity 组件的预制件，然后将其拖到 Inspector 中 MyNetworkManager 组件的 **treePrefab** 字段上。这样确保了在服务器生成树游戏对象的同时还在客户端上创建相同类型的游戏对象。

注册预制件可确保资源随场景一起加载，因此创建资源不会有卡顿或加载时间。

但是，要使脚本正常工作，还需要为服务器添加代码。请将以下代码添加到 **MyNetworkManager **脚本：

```
public void ServerListen() {
    NetworkServer.RegisterHandler(MsgType.Connect, OnServerConnect);
    NetworkServer.RegisterHandler(MsgType.Ready, OnClientReady);
    if (NetworkServer.Listen(4444))
        Debug.Log("Server started listening on port 4444");
}

// 当客户端准备好生成一些树时
void OnClientReady(NetworkMessage msg) {
    Debug.Log("Client is ready to start: " + msg.conn);
    NetworkServer.SetClientReady(msg.conn);
    SpawnTrees();
}

void SpawnTrees() {
    int x = 0;
    for (int i = 0; i < 5; ++i) {
        var treeGo = Instantiate(treePrefab, new Vector3(x++, 0, 0), Quaternion.identity);
        NetworkServer.Spawn(treeGo);
    }
}

void OnServerConnect(NetworkMessage msg) {
    Debug.Log("New client connected: " + msg.conn);
}

```

服务器不需要进行任何注册，因为它知道正在生成什么游戏对象（并在生成消息中发送资源 ID）。客户端需要能够查找游戏对象，因此必须在客户端上注册游戏对象。

在编写自己的 Network Manager 时，务必在服务器上调用生成命令之前让客户端准备好接收状态更新，否则不会发送状态更新。如果使用的是 Unity 的内置 Network Manager 组件，则会自动执行此操作。

对于更高级的用途，例如对象池或动态创建的资源，可使用 [ClientScene.RegisterSpawnHandler](../ScriptReference/Networking.ClientScene.RegisterSpawnHandler.html) 方法，该方法允许注册回调函数以实现客户端生成。如需查看这方面的示例，请参阅[自定义生成函数](UNetCustomSpawning.html)文档。

如果游戏对象具有类似于同步变量的网络状态，那么该状态与生成消息同步。在以下示例中，此脚本附加到树预制件：

```
using UnityEngine;
using UnityEngine.Networking;
class Tree : NetworkBehaviour {
    [SyncVar]
    public int numLeaves;
    public override void OnStartClient() {
        Debug.Log("Tree spawned with leaf count " + numLeaves);
    }
}
```

附加此脚本后，可更改 `numLeaves` 变量并修改 `SpawnTrees` 函数来查看其是否准确反映在客户端上：

```
void SpawnTrees() {
    int x = 0;
    for (int i = 0; i < 5; ++i) {
        var treeGo = Instantiate(treePrefab, new Vector3(x++, 0, 0), Quaternion.identity);
        var tree = treeGo.GetComponent<Tree>();
        tree.numLeaves = Random.Range(10,200);
        Debug.Log("Spawning leaf with leaf count " + tree.numLeaves);
        NetworkServer.Spawn(treeGo);
    }
}
```

请将 `Tree` 脚本附加到先前创建的 `treePrefab` 脚本来查看实际效果。

### 约束

* NetworkIdentity 必须位于可生成的预制件的根游戏对象上。否则，Network Manager 无法注册预制件。

* NetworkBehaviour 脚本必须与 NetworkIdentity 位于同一游戏对象上，而不是在子游戏对象上

## 游戏对象创建流程

生成游戏对象时发生的内部操作的实际流程如下：

* 将具有 Network Identity 组件的预制件注册为“可生成”。

* 从服务器上的预制件实例化游戏对象。

* 游戏代码在实例上设置初始值（请注意，此处施加的 3D 物理作用力不会立即生效）。

* 使用实例调用 `NetworkServer.Spawn()`。

* 通过在 [Network Behaviour](class-NetworkBehaviour.html) 组件上调用 `OnSerialize()` 来收集服务器上的实例上的 SyncVar 状态。

* 将 `MsgType.ObjectSpawn` 类型的网络消息发送到已连接的客户端，其中包括 SyncVar 数据。

* 在服务器上的实例上调用 `OnStartServer()`，并将 `isServer` 设置为 `true`

* 客户端接收 `ObjectSpawn` 消息并从已注册的预制件创建新实例。

* 通过在 Network Behaviour 组件上调用 [OnDeserialize()](../ScriptReference/Networking.NetworkBehaviour.OnDeserialize.html) 将 SyncVar 数据应用于客户端上的新实例。

* 在每个客户端上的实例上调用 `OnStartClient()`，并将 `isClient` 设置为`true`

* 随着游戏进程不断推进，对 SyncVar 值的更改会自动同步到客户端。此过程一直持续到游戏结束。

* 在服务器上的实例上调用 `NetworkServer.Destroy()`。

* 将 `MsgType.ObjectDestroy` 类型的网络消息发送到客户端。

* 在客户端上的实例上调用 `OnNetworkDestroy()`，然后销毁该实例。

### 玩家游戏对象

HLAPI 中的玩家游戏对象与非玩家游戏对象的工作方式略有不同。使用 Network Manager 生成玩家游戏对象的流程如下：

* 将具有 `NetworkIdentity` 的预制件注册为 `PlayerPrefab`

* 客户端连接到服务器

* 客户端调用 `AddPlayer()`，将 `MsgType.AddPlayer` 类型的网络消息发送到服务器

* 服务器接收消息并调用 `NetworkManager.OnServerAddPlayer()`

* 从服务器上的 PlayerPrefab 实例化游戏对象

* 在服务器上通过新玩家实例调用 `NetworkManager.AddPlayerForConnection()`

* 生成玩家实例 - 不必为玩家实例调用 `NetworkServer.Spawn()`。与普通生成一样，生成消息将发送到所有客户端。

* 将 `MsgType.Owner` 类型的网络消息发送到添加了玩家的客户端（仅限该客户端！）

* 原始客户端接收网络消息

* 在原始客户端上的玩家实例上调用 `OnStartLocalPlayer()`，并将 `isLocalPlayer` 设置为 true

请注意，在 `OnStartClient()` 之后调用 `OnStartLocalPlayer()`，因为只有在生成玩家游戏对象后所有权消息从服务器到达时才会发生此行为，因此在 `OnStartClient()` 中未设置 `isLocalPlayer`。

由于仅会为客户端的本地玩家游戏对象调用 `OnStartLocalPlayer`，因此对于仅应当对本地玩家完成的初始化，最好是在这里执行。这种情况下可能包括启用输入处理，并为玩家游戏对象启用摄像机追迹。

## 使用客户端授权生成游戏对象

要生成游戏对象并将这些游戏对象的授权分配给特定客户端，请使用 [NetworkServer.SpawnWithClientAuthority](../ScriptReference/Networking.NetworkServer.SpawnWithClientAuthority.html)，它会将要获得授权的客户端的 `NetworkConnection` 作为参数。

对于这些游戏对象，具有授权的客户端上的 `hasAuthority` 属性为 true，并在具有授权的客户端上调用 `OnStartAuthority()`。该客户端可为该游戏对象发出命令。在其他客户端上（以及主机上），`hasAuthority` 为 false。

使用客户端授权生成的对象必须在其 `NetworkIdentity` 中设置 `LocalPlayerAuthority`。

例如，可修改上面的树生成示例，从而让树具有如下所示的客户端授权（请注意，我们现在需要为该拥有客户端的连接传入一个 NetworkConnection 游戏对象）：

```
void SpawnTrees(NetworkConnection conn) {
    int x = 0;
    for (int i = 0; i < 5; ++i)
    {
        var treeGo = Instantiate(treePrefab, new Vector3(x++, 0, 0), Quaternion.identity);
        var tree = treeGo.GetComponent<Tree>();
        tree.numLeaves = Random.Range(10,200);
        Debug.Log("Spawning leaf with leaf count " + tree.numLeaves);
        NetworkServer.SpawnWithClientAuthority(treeGo, conn);
    }
}
```


可修改树脚本以将命令发送到服务器：

```
public override void OnStartAuthority() {
    CmdMessageFromTree("Tree with " + numLeaves + " reporting in");
}

[Command]
void CmdMessageFromTree(string msg) {
    Debug.Log("Client sent a tree message: " + msg);
}
```

请注意，不能只将 `CmdMessageFromTree` 调用添加到 `OnStartClient` 中，因为此时尚未设置授权，因此调用将失败。

