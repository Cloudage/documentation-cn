RPC 详细信息（旧版）
============================

*（对于新项目，应使用 [5.1 版中引入的新网络系统](UNet.html)。此信息适用于使用旧网络系统的旧版项目。）*

__远程过程调用__ (RPC) 可让您调用远程机器上的函数。调用 RPC 类似于调用普通函数，也几乎同样容易，但要理解一些重要的区别。


1.RPC 调用可以包含任意数量的参数，但所涉及的网络带宽将随参数的数量和大小而增加。为获得最佳性能，应将参数保持在最低限度。


1.与普通函数调用不同，RPC 需要一个额外的参数来表示 RPC 请求的接收方。有几种可能的 RPC 调用模式可以涵盖所有常见用例。例如，您可以轻松地在所有连接的计算机上、仅在服务器上、在除发送 RPC 调用的客户端外的所有客户端上或在特定客户端上调用 RPC 函数。

RPC 调用通常用于在游戏中的所有客户端上执行某种事件，或者专门在两方之间传递事件信息，但您可以发挥创造性，根据需要使用它们。例如，一个游戏只有在四个客户端连接后才能开始，该游戏的服务器可能在第四个客户端连接后立即向所有客户端发送 RPC 调用，从而开始游戏。特定玩家的客户端可以向每个人发送 RPC 调用来声明自己拾取了一个物品。服务器可以将 RPC 发送到特定客户端，以便在玩家连接后立即初始化玩家，例如，为其分配玩家编号、生成位置、团队颜色等。客户端可随后仅向服务器发送 RPC 以指定启动选项，例如玩家喜欢的颜色或购买的物品。


使用 RPC
----------


必须先将函数标记为 RPC，才能远程调用它。这是通过在脚本中使用 RPC 属性作为该函数的前缀实现的：

````
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	[RPC]
	void PrintText (string text)
	{
		Debug.Log(text);
	}
} 
````
_C# 脚本示例_

````
// 所有 RPC 调用均需要 @RPC 属性！
@RPC
function PrintText (text : String)
{
    Debug.Log(text);
}
````
_JS 脚本示例_

所有网络通信都由 NetworkView 组件处理，因此您必须将该组件附加到在脚本中声明 RPC 函数的对象，然后才能调用这些函数。


参数
----------


您可以使用以下变量类型作为 RPC 的参数：


* int
* float
* string
* NetworkPlayer
* NetworkViewID
* Vector3
* Quaternion

例如，以下代码使用单个字符串参数调用 RPC 函数：

````
using UnityEngine;
using UnityEngine.Network;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	NetworkView networkView;

	void Start() {
		networkView = new NetworkView ();
		networkView.RPC ("PrintText", RPCMode.All, "Hello world");
	}
}
````
_C# 脚本示例_

````
networkView.RPC ("PrintText", RPCMode.All, "Hello world");
````
_JS 脚本示例_

__RPC()__ 的第一个参数是要调用的函数的名称，而第二个参数确定了将调用该函数的目标。在本示例中，我们对连接到服务器的每个客户端执行 RPC 调用（但不会缓冲该调用来等待稍后连接的客户端；有关缓冲的更多详细信息，请参阅下文）。

前两个之后的所有参数都是要传递给 RPC 函数并通过网络发送的参数。在本示例中，“Hello World”将作为参数发送，并作为 PrintText 函数中的 text 参数传递。

您还可以访问一个额外的内部参数，即 [NetworkMessageInfo](../ScriptReference/NetworkMessageInfo.html) 结构，它用于存储其他信息，例如 RPC 调用的来源。此信息将自动传递，因此上面显示的 PrintText 函数可以声明为：

````
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	[RPC]
	void PrintText (string text, NetworkMessageInfo info)
	{
		Debug.Log(text + " from " + info.sender);
	}
}
````
_C# 脚本示例_

````
@RPC
function PrintText (text : String, info : NetworkMessageInfo)
{
    Debug.Log(text + " from " + info.sender);
}
````
_JS 脚本示例_

...调用方式与以前相同。

如上所述，网络视图必须附加到在脚本中包含 RPC 函数的游戏对象。如果要独占使用 RPC（即没有状态同步），则可以将网络视图的 __State Synchronization__ 设置为 __Off__。


RPC 缓冲区
----------


还可以缓冲 RPC 调用。缓冲的 RPC 调用将按照针对每个连接的新客户端发出调用的顺序进行存储和执行。这种方法可能非常有助于确保迟来的玩家获得游戏开始所需的所有信息。一种常见的情况是，每个加入游戏的玩家都应首先加载某一特定关卡。您可以将此关卡的详细信息发送给所有连接的玩家，但也可以缓冲这些信息以用于将来加入的玩家。通过这种做法，您可以确保新玩家收到关卡信息，就像该玩家从一开始就存在一样。

您还可以在必要时从 RPC 缓冲区中删除调用。继续参考上面的示例，游戏在新玩家加入时可能已经从起始关卡升级，因此您可以删除原来缓冲的 RPC 并发送新的 RPC 以请求新关卡。
