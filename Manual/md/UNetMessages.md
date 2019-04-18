# 网络消息

除了“高级”命令和 RPC 调用，还可以发送原始网络消息。

通过扩展一个名为 [MessageBase](../ScriptReference/Networking.MessageBase.html) 的类可以创建可序列化的网络消息类。此类包含 [Serialize](../ScriptReference/Networking.MessageBase.Serialize.html) 和 [Deserialize](../ScriptReference/Networking.MessageBase.Deserialize.html) 函数，这两个函数采用写入器和读取器对象。您可以自己实现这些函数，也可以依赖于由网络系统自动创建的代码生成实现。此基础类如下所示：

```
public abstract class MessageBase
{
    // 将读取器的内容反序列化到此消息中
    public virtual void Deserialize(NetworkReader reader) {}

    // 将此消息的内容序列化到写入器中
    public virtual void Serialize(NetworkWriter writer) {}
}
```

消息类可包含作为基本类型、结构和数组的成员，其中包括大部分常见的 Unity 引擎类型（比如 [Vector3](../ScriptReference/Vector3.html)）。消息类不能包含作为复杂类或通用容器的成员。切记，如果希望依赖于代码生成的实现，必须确保类型公开可见。

有一些内置消息类用于常见类型的网络消息：

* [EmptyMessage](../ScriptReference/Networking.NetworkSystem.EmptyMessage.html)

* [StringMessage](../ScriptReference/Networking.NetworkSystem.StringMessage.html)

* [IntegerMessage](../ScriptReference/Networking.NetworkSystem.IntegerMessage.html)

要发送消息，请对 [NetworkClient](../ScriptReference/Networking.NetworkClient.html)、[NetworkServer](../ScriptReference/Networking.NetworkServer.html) 和 [NetworkConnection](../ScriptReference/Networking.NetworkConnection.html) 类（这些类的工作方式相同）使用 `Send()` 方法。此方法采用消息 ID 和派生自 [MessageBase](../ScriptReference/Networking.MessageBase.html) 的消息对象。以下代码演示了如何使用其中一个内置消息类来发送和处理消息：

```
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Networking.NetworkSystem;

public class Begin : NetworkBehaviour
{
    const short MyBeginMsg = 1002;

    NetworkClient m_client;

    public void SendReadyToBeginMessage(int myId)
    {
        var msg = new IntegerMessage(myId);
        m_client.Send(MyBeginMsg, msg);
    }

    public void Init(NetworkClient client)
    {
        m_client = client;
        NetworkServer.RegisterHandler(MyBeginMsg, OnServerReadyToBeginMessage);
    }

    void OnServerReadyToBeginMessage(NetworkMessage netMsg)
    {
        var beginMessage = netMsg.ReadMessage<IntegerMessage>();
        Debug.Log("received OnServerReadyToBeginMessage " + beginMessage.value);
    }
}
```

声明一个自定义网络消息类并使用该类：

```
using UnityEngine;
using UnityEngine.Networking;

public class Scores : MonoBehaviour
{
    NetworkClient myClient;

    public class MyMsgType {
        public static short Score = MsgType.Highest + 1;
    };

    public class ScoreMessage : MessageBase
    {
        public int score;
        public Vector3 scorePos;
        public int lives;
    }

    public void SendScore(int score, Vector3 scorePos, int lives)
    {
        ScoreMessage msg = new ScoreMessage();
        msg.score = score;
        msg.scorePos = scorePos;
        msg.lives = lives;

        NetworkServer.SendToAll(MyMsgType.Score, msg);
    }

    // 创建客户端并连接到服务器端口
    public void SetupClient()
    {
        myClient = new NetworkClient();
        myClient.RegisterHandler(MsgType.Connect, OnConnected);
        myClient.RegisterHandler(MyMsgType.Score, OnScore);
        myClient.Connect("127.0.0.1", 4444);
    }

    public void OnScore(NetworkMessage netMsg)
    {
        ScoreMessage msg = netMsg.ReadMessage<ScoreMessage>();
        Debug.Log("OnScoreMessage " + msg.score);
    }

    public void OnConnected(NetworkMessage netMsg)
    {
        Debug.Log("Connected to server");
    }
}
```

请注意，在此源代码示例中，`ScoreMessage` 类没有序列化代码。Unity 会自动为此类生成序列化函数的主体。

## ErrorMessage 类

还有一个派生自 `MessageBase` 的 [ErrorMessage](../ScriptReference/Networking.NetworkSystem.ErrorMessage.html) 类。此类将传递到客户端和服务器上的错误回调。

ErrorMessage 类中的 [errorCode](../ScriptReference/Networking.NetworkSystem.ErrorMessage-errorCode.html) 对应于 [Networking.NetworkError](../ScriptReference/Networking.NetworkError.html) 枚举。

```
class MyClient
{
    NetworkClient client;
    
    void Start()
    {
        client = new NetworkClient();
        client.RegisterHandler(MsgType.Error, OnError);
    }
    
    void OnError(NetworkMessage netMsg)
    {
        var errorMsg = netMsg.ReadMessage<ErrorMessage>();
        Debug.Log("Error:" + errorMsg.errorCode);
    }
}

```
