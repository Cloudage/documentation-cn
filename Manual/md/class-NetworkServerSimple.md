NetworkServerSimple
==============

NetworkServerSimple 是一个高级 API (HLAPI) 类，可管理来自多个客户端的连接。NetworkServer 类负责处理游戏类型内容（比如生成、本地客户端和玩家管理器）而且有一个静态接口，而 NetworkServerSimple 类是一个纯网络服务器，无任何游戏相关功能。NetworkServerSimple 也没有任何静态接口或单例，所以在同一时间内，在一个进程中可存在多个实例。

NetworkServer 类在内部使用 NetworkServerSimple 的实例来进行连接管理。


属性
----------

|**_属性：_** ||**_功能：_** |
|:---|:---|:---|
|__connections__ ||与远程客户端的活动连接的集合。这是一个稀疏数组，其中的 NetworkConnect 对象位于其 connectionId 的索引位置。对于最近关闭的连接，此数组中可能存在 null 值。位于索引零的连接可能是来自本地客户端的连接。|
|__handlers__ ||已注册消息处理程序函数的集合。|
|__networkConnectionClass__ ||要为新连接创建的 NetworkConnection 对象的类型。|
|__hostTopology__ ||此服务器用来配置传输层的主机拓扑对象。
|__listenPort__ ||服务器执行监听的网络端口。|
|__serverHostId__ ||与此服务器实例相关联的传输层 hostId。|

