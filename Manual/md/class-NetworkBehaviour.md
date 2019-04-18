# NetworkBehaviour

NetworkBehaviour 脚本处理具有 [Network Identity](class-NetworkIdentity.html) 组件的游戏对象。这些脚本可以执行[高级 API](UNetUsingHLAPI.html) 函数，例如 [Command、ClientRPC](UNetActions.html)、[SyncEvent](../ScriptReference/Networking.SyncEventAttribute.html) 和 [SyncVar](UNetStateSync.html)。

对于 Unity 网络系统的服务器授权系统，服务器必须使用 [NetworkServer.Spawn](../ScriptReference/Networking.NetworkServer.Spawn.html) 函数通过 Network Identity 组件生成游戏对象。以这种方式生成游戏对象将为游戏对象分配 [NetworkInstanceId](../ScriptReference/Networking.NetworkInstanceId.html) 并在连接到服务器的客户端上创建游戏对象。

**注意：**这个组件不能直接添加到游戏对象。相反，必须创建一个继承自 `NetworkBehaviour`（而不是默认的 `MonoBehaviour`）的脚本，然后可以将脚本作为组件添加到游戏对象。

## 属性

|**属性**|**描述**|
|:---|:---|
|**`isLocalPlayer`**|如果此游戏对象是表示本地客户端上的玩家的游戏对象，则返回 true。|
|**`isServer`**|如果此游戏对象在服务器上运行并且已生成，则返回 true。|
|**`isClient`**|如果此游戏对象在客户端上并且已由服务器生成，则返回 true。|
|**`hasAuthority`**|如果此游戏对象是游戏对象的授权版本（这意味着此游戏对象是要同步更改的源），则返回 true。对于大多数游戏对象，此属性在服务器上返回 true。但是，如果 NetworkIdentity 上的 localPlayerAuthority 值为 true，则授权取决于该玩家的客户端，因此该值在该客户端上为 true，而不是在服务器上为 true。|
|**`netId`**|此游戏对象的唯一网络 ID。服务器在运行时分配此 ID。此属性对于该网络会话中的所有游戏对象而言是唯一的。|
|**`playerControllerId`**|与此 NetworkBehaviour 脚本关联的玩家的 ID。仅当对象为本地玩家时，此属性才有效。|
|**`connectionToServer`**|与附加到此游戏对象的 Network Identity 组件关联的 NetworkConnection。此属性只对客户端上的玩家对象有效。|
|**`connectionToClient`**|与附加到此游戏对象的 Network Identity 组件关联的 NetworkConnection。此属性只对服务器上的玩家游戏对象有效。|
|**`localPlayerAuthority`**|此值在 Network Identity 组件上设置，并可从 NetworkBehaviour 脚本访问，能够在脚本中方便访问。|

NetworkBehaviour 脚本具有以下功能：

* 同步变量

* 网络回调

* 服务器和客户端函数

* 发送命令

* 客户端 RPC 调用

* 联网事件

![](../uploads/Main/UNetDirections.jpg) 

### 同步变量

可以将 NetworkBehaviour 脚本的成员变量从服务器同步到客户端。服务器在此系统中具有权威性，因此仅在服务器到客户端的方向上进行同步。

应使用 [SyncVar](../ScriptReference/Networking.SyncVarAttribute.html) 属性将成员变量标记为同步变量。同步变量可以是任何基本类型（bool、byte、sbyte、char、decimal、double、float、int、uint、long、ulong、short、ushort 和 string），但不能是类、列表和其他集合。

```
public class SpaceShip : NetworkBehaviour
{
    [SyncVar]
    public int health;

    [SyncVar]
    public string playerName;
} 
```

当 `SyncVar` 的值在服务器上发生更改时，服务器会自动将新值发送给游戏中的所有就绪客户端，并更新这些客户端上的相应 `SyncVar` 值。当游戏对象生成时，客户端上将创建这些游戏对象，且游戏对象具有来自服务器的所有 `SyncVar` 属性的最新状态。

**注意：**要从客户端向服务器发出请求，必须使用**命令**，而不是使用同步变量。有关更多信息，请参阅有关发送命令的文档。

### 网络回调

可在 NetworkBehaviour 脚本上为各种网络事件调用一些内置的回调函数。这些函数是基类上的虚拟函数，因此您可以在自己的代码中重写这些函数，如下所示：

```
public class SpaceShip : NetworkBehaviour
{
    public override void OnStartServer()
    {
        // 禁用客户端内容
    }

    public override void OnStartClient()
    {
        // 注册客户端事件，启用效果
    }
}
```

内置回调包括：

* [OnStartServer](../ScriptReference/Networking.NetworkBehaviour.OnStartServer.html) - 在服务器上生成游戏对象时调用，或者在场景中为游戏对象启动服务器时调用

* [OnStartClient](../ScriptReference/Networking.NetworkBehaviour.OnStartClient.html) - 在客户端上生成游戏对象时调用，或者在客户端为场景中游戏对象连接到服务器时调用

* [OnSerialize](../ScriptReference/Networking.NetworkBehaviour.OnSerialize.html) - 调用此回调可收集要从服务器发送到客户端的状态

* [OnDeSerialize](../ScriptReference/Networking.NetworkBehaviour.OnDeserialize.html) - 调用此回调可在客户端上将状态应用于游戏对象

* [OnNetworkDestroy](../ScriptReference/Networking.NetworkBehaviour.OnNetworkDestroy.html) - 在服务器销毁游戏对象时在客户端上调用

* [OnStartLocalPlayer](../ScriptReference/Networking.NetworkBehaviour.OnStartLocalPlayer.html) - 在客户端上针对本地客户端上的玩家游戏对象调用（仅限本地客户端）

* [OnRebuildObservers](../ScriptReference/Networking.NetworkBehaviour.OnRebuildObservers.html) - 在重新构建游戏对象的观察者集合时在服务器上调用

* [OnSetLocalVisibility](../ScriptReference/Networking.NetworkBehaviour.OnSetLocalVisibility.html) - 当游戏对象的可见性对本地客户端而言更改时在客户端和/或服务器上调用

* [OnCheckObserver](../ScriptReference/Networking.NetworkBehaviour.OnCheckObserver.html) - 在服务器上调用以检查新客户端的可见性状态

请注意，在对等托管设置中，当其中一个客户端同时充当主机和客户端时，在同一个游戏对象上调用 `OnStartServer` 和 `OnStartClient`。这两个函数对于特定于客户端或服务器的操作非常有用，例如抑制对服务器的影响或设置客户端事件。

### 服务器和客户端函数

可以使用自定义属性在 NetworkBehaviour 脚本中标记成员函数，从而将它们指定为仅限服务器的函数或仅限客户端的函数。例如：

```
using UnityEngine;
using UnityEngine.Networking;

public class SimpleSpaceShip : NetworkBehaviour
{
    int health;

    [Server]
    public void TakeDamage( int amount)
    {
        // 将仅在服务器上有效
        health -= amount;
    }

    [ServerCallback]
    void Update()
    {
        // 引擎调用的回调 - 将仅在服务器上运行
    }


    [Client]
    void ShowExplosion()
    {
        // 将仅在客户端上运行
    }

    [ClientCallback]
    void Update()
    {
        // 引擎调用的回调 - 将仅在客户端上运行
    }
}
```

如果客户端未处于激活状态，则 `[Server]` 和 `[ServerCallback]` 会立即返回。同样，如果服务器未处于激活状态，`[Client]` 和 `[ClientCallback]` 会立即返回。

`[Server]` 和 `[Client]` 属性用于您自己的自定义回调函数。它们不会生成编译时错误，但如果在错误的作用域内调用它们，则会发出警告日志消息。

`[ServerCallback]` 和 `[ClientCallback]` 属性用于 Unity 自动调用的内置回调函数。这些属性不会导致生成警告。

有关更多信息，请参阅所讨论属性的 API 参考文档：

* [ClientAttribute](../ScriptReference/Networking.ClientAttribute.html)

* [ClientCallbackAttribute](../ScriptReference/Networking.ClientCallbackAttribute.html)

* [ServerAttribute](../ScriptReference/Networking.ServerAttribute.html)

* [ServerCallbackAttribute](../ScriptReference/Networking.ServerCallbackAttribute.html)

### 发送命令

要在服务器上执行代码，必须使用**命令**。高级 API 是服务器授权系统，因此要让客户端在服务器上触发某些代码，命令是唯一途径。

只有玩家游戏对象才能发送命令。

当客户端玩家游戏对象发送命令时，该命令将在服务器上的相应玩家游戏对象上运行。此路由过程自动发生，因此客户端无法为其他玩家发送命令。

要在代码中定义**命令**，必须编写一个具有以下元素的函数：

* 以 `Cmd` 开头的名称

* `[Command]` 属性

例如：

```
using UnityEngine;
using UnityEngine.Networking;

public class SpaceShip : NetworkBehaviour
{
    bool alive;
    float thrusting;
    int spin;

    [ClientCallback]
    void Update()
    {

        // 此代码在客户端上执行，用于收集输入
        int spin = 0;
        if (Input.GetKey(KeyCode.LeftArrow))
        {
            spin += 1;
        }
        if (Input.GetKey(KeyCode.RightArrow))
        {
            spin -= 1;
        }


        // 此行触发要在服务器上运行的代码
        CmdThrust(Input.GetAxis("Vertical"), spin);
    }

    [Command]
    public void CmdThrust(float thrusting, int spin)
    {   

        // 从下面调用 Update() 之后，

        // 此代码在服务器上执行。
        if (!alive)
        {
            this.thrusting = 0;
            this.spin = 0;
            return;
        }
            
        this.thrusting = thrusting;
        this.spin = spin;
    }

}
```

只需在客户端上正常调用函数即可调用命令。命令函数不在客户端上运行，而是在服务器上的相应玩家游戏对象上自动调用。

命令具有类型安全特性，具有内置的安全性并路由到玩家，还对参数使用有效的序列化机制来提高调用速度。

### 客户端 RPC 调用

客户端 RPC 调用是服务器游戏对象在客户端游戏对象上触发事件的一种方式。

 

客户端 RPC 调用不限于玩家游戏对象，并且可以在具有 [Network Identity](class-NetworkIdentity.html) 组件的任何游戏对象上调用。


要在代码中定义**客户端 RPC 调用**，必须编写一个符合以下条件的函数：

* 名称以 `Rpc` 开头

* 具有 `[ClientRPC]` 属性

例如：

```
using UnityEngine;
using UnityEngine.Networking;

public class SpaceShipRpc : NetworkBehaviour
{
    [ServerCallback]
    void Update()
    {

        // 这是在服务器上运行的代码
        int value = UnityEngine.Random.Range(0,100);
        if (value < 10)
        {
            // 这将在所有客户端上调用 RpcDoOnClient 函数
            RpcDoOnClient(value);
        }
    }

    [ClientRpc]
    public void RpcDoOnClient(int foo)
    {

        // 这是将在所有客户端上运行的代码
        Debug.Log("OnClient " + foo);
    }

}
```

### 联网事件

**联网事件**就像**客户端 RPC **调用，但它们不在游戏对象上调用函数，而是触发事件。

因此可以编写脚本以便在触发事件时注册回调。

要在代码中定义**联网事件**，必须编写一个同时符合以下两个条件的函数：

* 名称以 `Event` 开头

* 具有 `[SyncEvent]` 属性

可以使用事件来构建可由其他脚本扩展的强大联网游戏系统。以下示例显示了客户端上的效果脚本如何响应服务器上的战斗脚本生成的事件。

**SyncEvent** 是派生 **Command** 和 **ClientRPC** 调用的基类。您可以在自己的函数中使用 SyncEvent 属性来创建自己的事件驱动型网络游戏代码。通过使用 SyncEvent，您可以扩展 Unity 的 Multiplayer 功能，从而更好适应自己的编程模式。例如：


```
using UnityEngine;
using UnityEngine.Networking;

// 服务器脚本
public class MyCombat : NetworkBehaviour
{
    public delegate void TakeDamageDelegate(int amount);
    public delegate void DieDelegate();
    public delegate void RespawnDelegate();
    
    float deathTimer;
    bool alive;
    int health;

    [SyncEvent(channel=1)]
    public event TakeDamageDelegate EventTakeDamage;
    
    [SyncEvent]
    public event DieDelegate EventDie;
    
    [SyncEvent]
    public event RespawnDelegate EventRespawn;

    [ServerCallback]
    void Update()
    {

        // 检查是否到了重新生成 (Respawn) 的时间
        if (!alive)
        {
            if (Time.time > deathTimer)
            {
                Respawn();
            }
            return;
        }
    }

    [Server]
    void Respawn()
    {
        alive = true;


        // 将重新生成事件从服务器发送到所有客户端
        EventRespawn();
    }


    [Server]
    void EventTakeDamage(int amount)
    {
        if (!alive)
            return;
            
        if (health > amount) {
            health -= amount;
        }
        else
        {
            health = 0;
            alive = false;


            // 将死亡事件发送到所有客户端
            EventDie();
            deathTimer = Time.time + 5.0f;
        }
    }
}
```
