NetworkConnection
==============

NetworkConnection 是一个[高级 API](UNetUsingHLAPI.html) 类，可用于封装网络连接。(NetworkClient)[class-NetworkClient] 对象具有一个 `NetworkConnection`，而 [NetworkServer](class-NetworkServer.html) 具有多个连接：与每个客户端有一个连接。NetworkConnection 能够作为网络消息来发送字节数组或序列化对象。

属性
----------

|**_属性：_** ||**_功能：_** |
|:---|:---|:---|
|__hostId__ ||此连接的 [NetworkTransport](UNetUsingTransport.html) hostId。|
|__connectionId__ ||此连接的 `NetworkTransport` connectionId。|
|__isReady__ ||该标志用于控制是否将状态更新发送到此连接|
|__lastMessageTime__ ||在此连接上收到消息的最后时间。|
|__address__ ||此连接所连接到的端点的 IP 地址。 |
|__playerControllers__ ||已使用 AddPlayer() 添加的玩家的集合。 |
|__clientOwnedObjects__ ||此连接有授权的对象的集合。 |

NetworkConnection 类包含当数据发送到传输层或者从传输层接收数据时调用的虚拟函数。这些函数允许专用版本的 NetworkConnection 检查或修改此数据，甚至将数据路由到不同源。下面显示了这些函数，包括默认行为：

````
public virtual void TransportRecieve(byte[] bytes, int numBytes, int channelId)
{
	HandleBytes(bytes, numBytes, channelId);
}

public virtual bool TransportSend(byte[] bytes, int numBytes, int channelId, out byte error)
{
	return NetworkTransport.Send(hostId, connectionId, channelId, bytes, numBytes, out error);
} 
````

这些函数的一个用例是记录传入和传出数据包的内容。以下是派生自 NetworkConnection 的 DebugConnection 类的示例，此类用于将数据包的前 50 字节记录到控制台。要使用这样的类，请在 NetworkClient 或 NetworkServer 上调用 SetNetworkConnectionClass() 函数。

````
class DebugConnection : NetworkConnection
{
	public override void TransportRecieve(byte[] bytes, int numBytes, int channelId)
	{
		StringBuilder msg = new StringBuilder();
		for (int i = 0; i < numBytes; i++)
		{
			var s = String.Format("{0:X2}", bytes[i]);
			msg.Append(s);
			if (i > 50) break;
		}
		UnityEngine.Debug.Log("TransportRecieve h:" + hostId + " con:" + connectionId + " bytes:" + numBytes + " " + msg);

		HandleBytes(bytes, numBytes, channelId);
	}

	public override bool TransportSend(byte[] bytes, int numBytes, int channelId, out byte error)
	{
		StringBuilder msg = new StringBuilder();
		for (int i = 0; i < numBytes; i++)
		{
			var s = String.Format("{0:X2}", bytes[i]);
			msg.Append(s);
			if (i > 50) break;
		}
		UnityEngine.Debug.Log("TransportSend    h:" + hostId + " con:" + connectionId + " bytes:" + numBytes + " " + msg);

		return NetworkTransport.Send(hostId, connectionId, channelId, bytes, numBytes, out error);
	}
}
````
