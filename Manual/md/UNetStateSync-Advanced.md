#高级状态同步

在大多数情况下，使用 SyncVar 足以让游戏脚本将其状态序列化到客户端。但是在某些情况下，可能需要更复杂的序列化代码。本页面仅适用于高级开发人员，他们需要自定义同步解决方案来解决 Unity 常规 SyncVar 功能无法满足的情况。

## **自定义序列化函数**

要执行自己的自定义序列化，可在要用于 SyncVar 序列化的 NetworkBehaviour 上实现虚拟函数。这些函数为：

```

public virtual bool OnSerialize(NetworkWriter writer, bool initialState); 

```

```
public virtual void OnDeSerialize(NetworkReader reader, bool initialState); 
```

使用 `initialState` 标志可区分游戏对象第一次序列化和可发送增量更新的时间。第一次将游戏对象发送到客户端时，该游戏对象必须包括完整状态快照，但后续更新可以通过仅包括增量更改来节省带宽。请注意，`initialState` 为 true 时不会调用 SyncVar 挂钩函数，仅对增量更新调用这些函数。

如果一个类具有 SyncVar，则会自动将这些函数的实现添加到该类中，这意味着具有 SyncVar 的类不能同时具有自定义序列化函数。

`OnSerialize` 函数应返回 true 来表明应该发送更新。如果返回 true，则该脚本的脏位设置为零。如果返回 false，则脏位不变。该方法允许对脚本的多个更改随时间累积，并在系统就绪时发送，而不是按帧发送。

## **序列化流程**

附加了 Network Identity 组件的游戏对象可具有从 `NetworkBehaviour` 派生的多个脚本。序列化这些游戏对象的流程如下：

在服务器上：

* 每个 `NetworkBehaviour` 都有一个脏掩码。此掩码在 `OnSerialize` 中作为 `syncVarDirtyBits` 提供

* 为 `NetworkBehaviour` 脚本中的每个 SyncVar 都分配脏掩码中的一个位。

* 更改 SyncVar 的值会导致在脏掩码中设置该 SyncVar 的位

* 或者，调用 `SetDirtyBit()` 直接写入脏掩码

* 在服务器更新过程中检查服务器上的 NetworkIdentity 游戏对象

* 如果 `NetworkIdentity` 上的任何 `NetworkBehaviours` 处于脏状态，则为该游戏对象创建一个 `UpdateVars` 数据包

* 通过在游戏对象上的每个 `NetworkBehaviour` 上调用 `OnSerialize` 来填充 `UpdateVars` 数据包

* 未处于脏状态的 `NetworkBehaviours` 向数据包中对应的脏位写入零

* 处于脏状态的 `NetworkBehaviours` 会写入其脏掩码，然后写入发生变化的 SyncVar 的值

* 如果 `OnSerialize` 针对 `NetworkBehaviour` 返回 true，则会重置该 `NetworkBehaviour` 的脏掩码，因此在值变化之前不会再次发送。

* 将 `UpdateVars` 数据包发送到正在观察游戏对象的就绪客户端

在客户端上：

* 接收到游戏对象的 `UpdateVars packet`

* 为游戏对象上的每个 `NetworkBehaviour` 脚本调用 `OnDeserialize` 函数

* 游戏对象上的每个 `NetworkBehaviour` 脚本都会读取一个脏掩码。

* 如果某个 `NetworkBehaviour` 的脏掩码为零，则 `OnDeserialize` 函数会返回而不再读取

* 如果脏掩码为非零值，则 `OnDeserialize` 函数会读取与设置的脏位对应的 SyncVar 值

* 如果存在 SyncVar 挂钩函数，则会使用从流中读取的值调用这些函数。

因此，对于以下脚本：

```

public class data : NetworkBehaviour
{

    [SyncVar]
    public int int1 = 66;

    [SyncVar]
    public int int2 = 23487;

    [SyncVar]
    public string MyString = "Example string";
}

```

以下代码示例演示了生成的 `OnSerialize` 函数：

```

public override bool OnSerialize(NetworkWriter writer, bool forceAll)
{
    if (forceAll)
    {
        // 第一次将游戏对象发送到客户端时，发送所有数据（并且没有脏位）
        writer.WritePackedUInt32((uint)this.int1);
        writer.WritePackedUInt32((uint)this.int2);
        writer.Write(this.MyString);
        return true;
    }
    bool wroteSyncVar = false;
    if ((base.get_syncVarDirtyBits() & 1u) != 0u)
    {
        if (!wroteSyncVar)
        {
            // 如果这是写入的第一个 SyncVar，则写入脏位
            writer.WritePackedUInt32(base.get_syncVarDirtyBits());
            wroteSyncVar = true;
        }
        writer.WritePackedUInt32((uint)this.int1);
    }
    if ((base.get_syncVarDirtyBits() & 2u) != 0u)
    {
        if (!wroteSyncVar)
        {
            // 如果这是写入的第一个 SyncVar，则写入脏位
            writer.WritePackedUInt32(base.get_syncVarDirtyBits());
            wroteSyncVar = true;
        }
        writer.WritePackedUInt32((uint)this.int2);
    }
    if ((base.get_syncVarDirtyBits() & 4u) != 0u)
    {
        if (!wroteSyncVar)
        {
            // 如果这是写入的第一个 SyncVar，则写入脏位
            writer.WritePackedUInt32(base.get_syncVarDirtyBits());
            wroteSyncVar = true;
        }
        writer.Write(this.MyString);
    }

    if (!wroteSyncVar)
    {
        // 如果没有写入 SyncVar，则写入零脏位
        writer.WritePackedUInt32(0);
    }
    return wroteSyncVar;
}
```

以下代码示例演示了 `OnDeserialize` 函数：

```

public override void OnDeserialize(NetworkReader reader, bool initialState)
{
    if (initialState)
    {
        this.int1 = (int)reader.ReadPackedUInt32();
        this.int2 = (int)reader.ReadPackedUInt32();
        this.MyString = reader.ReadString();
        return;
    }
    int num = (int)reader.ReadPackedUInt32();
    if ((num & 1) != 0)
    {
        this.int1 = (int)reader.ReadPackedUInt32();
    }
    if ((num & 2) != 0)
    {
        this.int2 = (int)reader.ReadPackedUInt32();
    }
    if ((num & 4) != 0)
    {
        this.MyString = reader.ReadString();
    }
} 
```

如果 `NetworkBehaviour` 的某个基类中也有序列化函数，则还应调用基类函数。

请注意，为游戏对象状态更新创建的 `UpdateVar` 数据包可能会在发送到客户端之前在缓冲区中聚合，因此单个传输层数据包可能包含多个游戏对象的更新。

