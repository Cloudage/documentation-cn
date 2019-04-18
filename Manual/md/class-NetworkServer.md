NetworkServer
==============

NetworkServer 是一个高级 API 类，可管理来自多个客户端的连接。


属性
----------

|**_属性：_** |**_功能：_** |
|:---|:---|
|active           |检查服务器是否已启动。|
|connections      |来自客户端的所有当前连接的列表。|
|dontListen       |如果启用此属性，则服务器将不会在常规网络端口上监听传入连接。|
|handlers         |向服务器注册的消息处理程序的字典。|
|hostTopology     |服务器使用的主机拓扑。|
|listenPort       |服务器执行监听的端口。|
|localClientActive|如果本地客户端当前在服务器上处于活动状态，则为 true。|
|localConnections |服务器上的本地连接的列表。|
|maxDelay         |在连接上发送数据包之前的最大延迟。|
|networkConnectionClass|创建新网络连接时要使用的类。|
|numChannels      |为网络配置的通道数量。
|objects          |这是已在服务器上生成的联网对象的字典。|
|serverHostId     |此服务器使用的传输层 hostId。|
|useWebSockets    |此属性可使服务器监听 WebSocket 连接，而不是正常的传输层连接。|
