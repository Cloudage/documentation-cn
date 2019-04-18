#NetworkReader 和 NetworkWriter 序列化程序

使用 [NetworkReader](../ScriptReference/Networking.NetworkReader.html) 和 [NetworkWriter](../ScriptReference/Networking.NetworkWriter.html) 类可将数据写入到字节流。

多玩家[高级 API](UNetUsingHLAPI.html) 是用这些类构建的，并广泛使用这些类。但是，如果要实现自己的自定义传输功能，可直接使用这些类。这些类具有用于大量 Unity 类型的特定序列化函数（有关类型的完整列表，请参阅 [NetworkWriter.Write](../ScriptReference/Networking.NetworkWriter.Write.html)）。

要使用这些类，请创建一个写入器实例，并将单独变量写入到其中。这些变量在内部序列化为字节数组，并可通过网络来发送该数组。在接收端，字节数组的读取器实例按照完全相同的写入顺序对变量进行回读。

这可与 [MessageBase](../ScriptReference/Networking.MessageBase.html) 类结合使用来生成包含序列化网络消息的字节数组。

```
void SendMessage(short msgType, MessageBase msg, int channelId)
{
    // 将消息写入本地缓冲区
    NetworkWriter writer = new NetworkWriter();
    writer.StartMessage(msgType);
    msg.Serialize(writer);
    writer.FinishMessage();

    myClient.SendWriter(writer, channelId);
}
```

此消息经过正确的格式设置，从而可为其调用消息处理程序函数。

## 将 NetworkReader 和 NetworkWriter 类与 NetworkServerSimple 和 NetworkClient 类结合使用

以下代码示例是很低级的演示，使用了高级 API 中的最低级类来设置连接。

以下是用于将客户端和服务器连接在一起的代码：

```
using UnityEngine;
using UnityEngine.Networking;
public class Serializer : MonoBehaviour {
    NetworkServerSimple m_Server;
    NetworkClient m_Client;
    const short k_MyMessage = 100;

    // 使用这样的服务器实例时，必须手动将其送入
    void Update() {
        if (m_Server != null)
            m_Server.Update();
    }

    void StartServer() {
        m_Server = new NetworkServerSimple();
        m_Server.RegisterHandler(k_MyMessage, OnMyMessage);
        if (m_Server.Listen(5555))
            Debug.Log("Started listening on 5555");
    }

    void StartClient() {
        m_Client = new NetworkClient();
        m_Client.RegisterHandler(MsgType.Connect, OnClientConnected);
        m_Client.Connect("127.0.0.1", 5555);
    }

    void OnClientConnected(NetworkMessage netmsg) {
        Debug.Log("Client connected to server");
        SendMessage();
    }
}
```

代码的下一部分使用网络读取器和网络写入器来发送消息，但会使用这些类中内置的消息处理程序：

void SendMessage() {
    NetworkWriter writer = new NetworkWriter();
    writer.StartMessage(k_MyMessage);
    writer.Write(42);
    writer.Write("What is the answer");
    writer.FinishMessage();
    m_Client.SendWriter(writer, 0);
}

void OnMyMessage(NetworkMessage netmsg) {
    Debug.Log("Got message, size=" + netmsg.reader.Length);
    var someValue = netmsg.reader.ReadInt32();
    var someString = netmsg.reader.ReadString();
    Debug.Log("Message value=" + someValue + " Message string='" + someString + "'");
}

为消息处理程序设置消息时，应始终使用 `NetworkWriter.StartMessage()`（包括消息类型 ID）和 NetworkWriter.FinishMessage() 调用。如果不使用字节数组，可跳过该步骤。
