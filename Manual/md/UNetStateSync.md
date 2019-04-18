# 状态同步

状态同步是指对值（例如联网游戏对象上的脚本包含的整数、浮点数、字符串和布尔值）进行同步。

状态同步的方向是从服务器到远程客户端。*本地*客户端没有序列化的数据。本地客户端与服务器共享场景，因此不需要此数据。但是，在本地客户端上会调用 SyncVar 挂钩。

数据不会以相反方向（远程客户端到服务器）同步。若要进行此方向的同步，需要使用[命令](UNetActions.html)。

## SyncVar

SyncVar 是从 NetworkBehaviour 继承的脚本变量，这些变量从服务器同步到客户端。生成游戏对象时，或者新玩家加入正在进行的游戏时，会向其发送其可见的联网对象上所有 SyncVar 的最新状态。使用 `[SyncVar]` 自定义属性可指定脚本中要同步的变量，如下所示：

class Player : NetworkBehaviour
{

    [SyncVar]
    int health;

    public void TakeDamage(int amount)
    {
        if (!isServer)
            return;

        health -= amount;
    }
}

在调用 OnStartClient() 之前，SyncVar 的状态将应用于客户端上的游戏对象，因此 OnStartClient() 内的对象状态始终是最新的。

SyncVar 可以是基本类型，例如整数、字符串和浮点数。它们也可以是 Unity 类型，例如 Vector3 和用户定义的结构，但 SyncVar 结构的更新将作为整体更新进行发送，而不是在结构中的字段发生更改的情况下发送增量更改。一个 NetworkBehaviour 脚本最多可包含 32 个 SyncVar，包括 SyncList（请参阅下一部分）。

当 SyncVar 的值更改时，服务器会自动发送 SyncVar 更新，因此您无需跟踪它们何时更改或自己发送有关更改的信息。

## SyncList

SyncVar 包含值，而 SyncList 包含值列表。SyncList 内容与 SyncVar 状态一起包含在初始状态更新中。由于 SyncList 是一个同步其自身内容的类，因此 SyncList 不需要 SyncVar 属性。以下类型的 SyncList 可用于基本类型：

* SyncListString

* SyncListFloat

* SyncListInt

* SyncListUInt

* SyncListBool

此外还有 SyncListStruct 可用于同步您自己的结构类型的列表。使用 SyncListStruct 时，选择使用的结构类型可以包含基本类型、数组和常见 Unity 类型的成员。这些结构不能包含复杂类或通用容器，并且只序列化其中的公共变量。

SyncList 有一个名为“回调”(Callback) 的 SyncListChanged 委托，可在列表内容发生变化时通知客户端。应使用所发生的操作类型以及该操作的目标项的索引来调用此委托。

```

public class MyScript : NetworkBehaviour
{
    public struct Buf
    {
        public int id;
        public string name;
        public float timer;
    };
            
    public class TestBufs : SyncListStruct<Buf> {}
    TestBufs m_bufs = new TestBufs();
    
    void BufChanged(Operation op, int itemIndex)
    {
        Debug.Log("buf changed:" + op);
    }
    
    void Start()
    {
        m_bufs.Callback = BufChanged;
    }
}
```
