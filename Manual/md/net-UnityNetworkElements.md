Unity 中的网络元素（旧版）
============================

*（对于新项目，应使用 [5.1 版中引入的新网络系统](UNet.html)。此信息适用于使用旧网络系统的旧版项目。）*

Unity 的原生网络架构支持上一页中讨论的所有内容。服务器创建和客户端连接、在连接的客户端之间共享数据、确定哪个玩家控制哪些对象以及穿透网络配置变化都是直接支持的功能。本页面将引导您在 Unity 背景中实现这些网络实践操作。


创建服务器
-----------------


在开始玩网络游戏之前，必须确定要进行通信的不同计算机。为此，必须创建一个服务器。此服务器可以是运行游戏的机器，也可以是不参与游戏的专用机器。要创建服务器，只需从脚本中调用 [Network.InitializeServer()](../ScriptReference/Network.InitializeServer.html)。希望以客户端身份连接到现有服务器时，应调用 [Network.Connect()](../ScriptReference/Network.Connect.html)。

通常，您会发现，熟悉整个 [Network 类](../ScriptReference/Network.html)将非常有用。


使用网络视图进行通信
---------------------------------


__网络视图 (Network View)__ 是在网络上发送数据的组件。网络视图使游戏对象能够使用 RPC 调用或状态同步 (State Synchronization) 来发送数据。您使用网络视图的方式将决定游戏网络行为将如何运作。网络视图几乎没有选项，但它们对于网络游戏非常重要。

有关使用网络视图的更多信息，请阅读[网络视图指南页面](net-NetworkView.html)和[组件参考页面](class-NetworkView.html)。


远程过程调用
----------------------


远程过程调用 (RPC) 是在脚本中声明的函数，这些脚本附加到包含网络视图的游戏对象。
网络视图必须指向包含 RPC 函数的脚本。然后，可从该游戏对象中的任何脚本调用 RPC 函数。

有关在 Unity 中使用 RPC 的更多信息，请阅读 [RPC 详细信息页面](net-RPCDetails.html)。


状态同步
---------------------


状态同步是指在所有游戏客户端之间持续共享数据。这样，玩家的位置可以在所有客户端上同步，因此该位置看起来是在本地控制的，但数据实际是通过网络传送的。要在游戏对象中同步状态，只需要向该对象添加 NetworkView 并告诉它要观察什么。然后，观察到的数据将在游戏中的所有客户端之间同步。

有关在 Unity 中使用状态同步功能的更多信息，请阅读[状态同步页面](net-StateSynchronization.html)。


Network.Instantiate()
---------------------


__Network.Instantiate()__ 让您以自然而简单的方式在所有客户端上实例化预制件。本质上，这是 __Instantiate()__ 调用，但它在所有客户端上执行实例化。

在内部，Network.Instantiate 只是一个在所有客户端上（也在本地）执行的经过缓冲的 RPC 调用。它会分配 NetworkViewID 并将其分配给实例化的预制件，以确保其在所有客户端之间正确同步。

有关更多信息，请阅读[网络实例化](net-NetworkInstantiate.html)页面。


NetworkLevelLoad()
------------------


处理共享数据、客户端玩家状态和加载关卡可能有点复杂。[网络加载关卡](net-NetworkLevelLoad.html)页面提供了一个管理此任务的有用示例。


主服务器
-------------


__主服务器__可帮助您匹配游戏。启动服务器时，您将连接到主服务器，服务器会提供所有活动服务器的列表。

__主服务器__是服务器和客户端的集合位置，在此处会通告服务器，而兼容客户端可以连接到正在运行的游戏。因此避免了使用所有相关方的 IP 地址不断进行尝试的必要。主服务器甚至还可以帮助用户托管游戏，无需使用在正常情况下需要用到的路由器。它可以帮助客户端绕过服务器的防火墙，并访问通常无法通过公共 Internet 访问的私有 IP 地址。这是在协调程序 (facilitator) 的帮助下实现的，协调程序可_促成_连接的建立。

有关更多信息，请阅读[主服务器页面](net-MasterServer.html)。


最小化带宽
--------------------


使用最少带宽使游戏正常运行至关重要。您可以了解不同的数据发送方法、如何决定发送_什么_或_何时_发送以及其他技巧。

有关减少带宽使用量的提示和技巧，请阅读[最小化带宽页面](net-MinimizingBandwidth.html)。


调试网络游戏
-------------------------


Unity 提供了多种工具来帮助您调试网络游戏。


1.[Network Manager](class-NetworkManager.html) 可用于记录所有传入和传出的网络流量。
1.通过有效使用检视面板和 Hierarchy 视图，您可以跟踪对象的创建并检查视图 ID 等。
1.您可以在同一台计算机上启动 Unity 两次，并在每一个中打开不同的项目。在 Windows 上，只需启动另一个 Unity 实例并从项目向导中打开项目即可实现此目的。在 Mac OS X 上，可从终端打开多个 Unity 实例，并可指定 __-projectPath__ 参数：
    /Applications/Unity/Unity.app/Contents/MacOS/Unity -projectPath "/Users/MyUser/MyProjectFolder/"
    /Applications/Unity/Unity.app/Contents/MacOS/Unity -projectPath "/Users/MyUser/MyOtherProjectFolder/"


在调试网络时，请确保让玩家在后台运行，例如，如果您有两个实例同时运行，其中一个实例将没有焦点。这种情况下将破坏网络循环并导致不良结果。您可以在编辑器的 Edit &gt; Project Settings &gt; Player 中启用此功能，或使用 [Application.runInBackground](../ScriptReference/Application-runInBackground.html) 进行启用。
