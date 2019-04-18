#自定义网络可见性

内置的 [Network Proximity Checker](class-NetworkProximityChecker.html) 组件是用于确定游戏对象网络可见性的内置默认组件。但是，此组件仅提供基于距离的检查。有时可能希望使用其他类型的可见性检查，例如基于网格的规则、视线测试、导航路径测试或适合具体游戏的任何其他类型的测试。

为此，可实现与 Network Proximity Checker 等效的自定义功能。为实现此目的，需要了解 Network Proximity Checker 的工作原理。请参阅 Editor 内的 [Network Proximity Checker 组件](class-NetworkProximityChecker.html)和 [NetworkProximityChecker API](../ScriptReference/Networking.NetworkProximityChecker.html) 的相关文档。

Network Proximity Checker 是使用 Unity 多玩家 HLAPI 的公共**可见性接口**实现的。通过同样使用此接口，您可以实现所需的任何类型的可见性规则。每个 [NetworkIdentity](../ScriptReference/Networking.NetworkIdentity.html) 都会跟踪可看见该游戏对象的玩家集合。可看见 NetworkIdentity 游戏对象的玩家称为 NetworkIdentity 的“观察者”。

Network Proximity Checker 以固定间隔（使用 Inspector 中的“**Vis Update Interval**”值进行设置）调用 [Network Identity 组件](class-NetworkIdentity.html)上的 [RebuildObservers](../ScriptReference/Networking.NetworkIdentity.RebuildObservers.html)** **方法，以便在每个玩家移动时更新玩家的网络可见游戏对象的集合。

在 `NetworkBehaviour`** **类（联网脚本继承自该类）上，有一些用于确定可见性的虚拟函数。如下：

* [OnCheckObserver](../ScriptReference/Networking.NetworkBehaviour.OnCheckObserver.html) - 当新玩家进入游戏时，对每个联网游戏对象在服务器上调用此方法。如果返回 true，则将该玩家添加到对象的观察者中。`NetworkProximityChecker` 方法在其对此函数的实现中执行简单的距离检查，并使用 `Physics.OverlapSphere()` 来查找此对象的可见性距离内的玩家。

* [OnRebuildObservers](../ScriptReference/Networking.NetworkBehaviour.OnRebuildObservers.html) - 调用 `RebuildObservers` 时在服务器上调用此方法。此方法期望用可以看到对象的玩家填充观察者集。然后，NetworkServer 根据新旧可见性集之间的差异来处理 `ObjectHide` 和 `ObjectSpawn` 消息的发送。

为了检查任何给定的联网游戏对象是否为玩家，可以检查其 `NetworkIdentity` 是否存在有效的 [connectionToClient](../ScriptReference/Networking.NetworkIdentity-connectionToClient.html)。例如：

```

    var hits = Physics.OverlapSphere(transform.position, visRange);
        foreach (var hit in hits)
        {
            //（如果游戏对象具有 connectionToClient，则表示它是玩家）
            var uv = hit.GetComponent<NetworkIdentity>();
            if (uv != null && uv.connectionToClient != null)
            {
                observers.Add(uv.connectionToClient);
            }
        }
```
