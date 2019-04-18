网络实例化（旧版）
============================

*（对于新项目，应使用 [5.1 版中引入的新网络系统](UNet.html)。此信息适用于使用旧网络系统的旧版项目。）*

[Network.Instantiate](../ScriptReference/Network.Instantiate.html) 函数提供了一种在所有客户端上实例化预制件的简单方法，类似于单个客户端上 [Object.Instantiate](../ScriptReference/Object.Instantiate.html) 的效果。实例化客户端是控制对象的客户端（即只能从客户端实例上运行的脚本访问 Input 类），但更改将在网络上反映。


__Network.Instantiate()__ 的参数列表如下所示：



````
static function Instantiate (prefab : Object, position : Vector3, rotation : Quaternion, group : int) : Object


````

与 Object.Instantiate 一样，前三个参数描述了要实例化的预制件及其所需的位置和旋转。_group_ 参数用于定义对象子组以控制消息的过滤，如果不需要过滤，则可以设置为零（请参阅下面的_通信组_部分）。

技术细节
-----------------


在幕后，网络实例化是围绕一个 RPC 调用（其中包含预制件的标识符以及位置和其他细节）构建的。该 RPC 调用始终与其他 RPC 调用一样进行缓冲，因此实例化后的对象将在新客户端连接时显示在新客户端上。有关缓冲的更多详细信息，请参阅 [RPC](net-RPCDetails.html) 页面。


通信组
--------------------


通信组可用于选择将要接收特定消息的客户端。例如，两个联网的玩家可能处于游戏世界的不同区域，并可能永远无法见到彼此。因此没有理由在两个玩家客户端之间同步游戏状态，但您可能仍希望允许他们之间的聊天通信。在这种情况下，需要限制实例化的是游戏对象，而不是可实现聊天功能的对象，因此它们将放在不同的组中。
