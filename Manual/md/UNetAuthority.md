# 网络授权

服务器和客户端都可以管理游戏对象的行为。“授权”的概念是指如何以及在何处管理游戏对象。

### 服务器授权

使用 HLAPI 的 Unity 联网游戏中的默认授权状态是：服务器对所有不代表玩家的游戏对象都具有授权。举例来说，这意味着服务器将管理所有可收集物品、移动平台、NPC 以及玩家可以交互的游戏任何其他部分的控制权，并且玩家游戏对象对其所有者的客户端具有授权（意味着客户端管理它们的行为）。

### 本地授权

本地授权（有时称为客户端授权）表示本地客户端具有对特定联网游戏对象的授权控制。这与默认状态形成对比，默认状态是服务器具有对联网游戏对象的授权控制。

除了 `isLocalPlayer**`** 之外，还可以选择让玩家游戏对象拥有“本地授权”。这意味着其所有者客户端上的玩家游戏对象对其自身负责（或具有授权）。这对于控制移动特别有用；这意味着每个客户端对于自身玩家游戏对象如何受到控制都具有授权。

要在游戏对象上启用本地玩家授权，请勾选 Network Identity 组件的 **Local Player Authority** 复选框。[Network Transform](class-NetworkTransform.html) 组件会使用此“授权”设置，并将移动信息从客户端发送到其他客户端（如果已进行此设置）。

有关通过脚本实现本地玩家授权的信息，请参阅关于 [NetworkIdentity](../ScriptReference/Networking.NetworkIdentity.html) 和 [localPlayerAuthority](../ScriptReference/Networking.NetworkIdentity-localPlayerAuthority.html) 的脚本 API 参考文档。

![此图显示了 Server 授权下的 Enemy 对象。Enemy 出现在 Client 1 和 Client 2 上，但 Server 负责其位置、移动和行为](../uploads/Main/NetworkAuthority.png)

使用 [NetworkIdentity.hasAuthority](../ScriptReference/Networking.NetworkIdentity-hasAuthority.html) 属性可确定游戏对象是否具有本地授权（为方便起见，也可在 `NetworkBehaviour` 上访问）。非玩家游戏对象在服务器上具有授权，而设置了 **localPlayerAuthority** 的玩家游戏对象在其所有者的客户端上具有授权。

### 非玩家游戏对象的本地（客户端）授权

针对非玩家游戏对象可以获取其客户端授权。有两种方式可以做到这一点。一种是使用 [NetworkServer.SpawnWithClientAuthority](../ScriptReference/Networking.NetworkServer.SpawnWithClientAuthority.html) 生成游戏对象，并传递客户端的网络连接以获得所有权。另一种是使用 [NetworkIdentity.AssignClientAuthority](../ScriptReference/Networking.NetworkIdentity.AssignClientAuthority.html) 与客户端的网络连接来获取所有权。

为客户端分配授权会导致 Unity 在游戏对象上的每个 `NetworkBehaviour` 上调用 [OnStartAuthority()](../ScriptReference/Networking.NetworkBehaviour.OnStartAuthority.html)，并将 `hasAuthority`** **属性设置为 true。在其他客户端上，`hasAuthority`** **属性仍为 false。具有客户端授权的非玩家游戏对象可以像玩家一样发送[命令](UNetActions.html)。这些[命令](UNetActions.html)对服务器的游戏对象实例运行，而不是对与连接相关联的玩家运行。

如果希望非玩家游戏对象具有客户端授权，则必须在这些游戏对象的 Network Identity 组件上启用 **localPlayerAuthority**。下面的示例将生成一个游戏对象并将授权分配给生成此游戏对象的玩家的客户端。

```
[Command]
void CmdSpawn()
{
    var go = (GameObject)Instantiate(otherPrefab, transform.position + new Vector3(0,1,0), Quaternion.identity);
    NetworkServer.SpawnWithClientAuthority(go, connectionToClient);
}
```

## 网络上下文属性

`NetworkBehaviour` 类包含的属性允许脚本随时了解联网游戏对象的上下文。

* [isServer](../ScriptReference/Networking.NetworkBehaviour-isServer.html) - 如果游戏对象位于服务器（或主机）上并且已生成，则为 true。

* [isClient](../ScriptReference/Networking.NetworkBehaviour-isClient.html) - 如果游戏对象位于客户端上并且是由服务器创建的，则为 true。

* [isLocalPlayer](../ScriptReference/Networking.NetworkBehaviour-isLocalPlayer.html) - 如果游戏对象是此客户端的玩家游戏对象，则为 true。

* [hasAuthority](../ScriptReference/Networking.NetworkBehaviour-hasAuthority.html) - 如果游戏对象归本地进程所有，则为 true

要查看这些属性，请选择要检查的游戏对象，然后在 Inspector 窗口中查看 [NetworkBehaviour](class-NetworkBehaviour.html) 脚本组件的预览窗口。可以使用这些属性的值来根据运行脚本的上下文执行代码。
