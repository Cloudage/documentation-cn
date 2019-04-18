主服务器（旧版）
============================

*（对于新项目，应使用 [5.1 版中引入的新网络系统](UNet.html)。此信息适用于使用旧网络系统的旧版项目。）*

主服务器是可将游戏实例与想要连接它们的玩家客户端联系起来的集合位置。该服务器还可以隐藏端口号和 IP 地址，并执行设置网络连接时所需的其他技术任务，例如防火墙处理和 NAT 穿透。

每个运行的游戏实例都要向主服务器提供__游戏类型__。当玩家连接主服务器并从主服务器查询匹配的__游戏类型__时，服务器将发回正在运行的游戏列表以及每个游戏中的玩家数量及是否需要密码来参与游戏。用于交换此数据的两个函数为服务器的 [MasterServer.RegisterHost()](../ScriptReference/MasterServer.RegisterHost.html) 和玩家客户端的 [MasterServer.RequestHostList()](../ScriptReference/MasterServer.RequestHostList.html)。

调用 __RegisterHost__ 时，必须为所要注册的主机传递三个参数：_gameTypeName_（前面提到的__游戏类型__）、_gameName_ 和 _comment_。__RequestHostList__ 将所要连接的主机的 _gameTypeName_ 作为参数。然后，该类型的所有已注册主机将返回到请求客户端。此操作为异步操作，全部到达后可通过 __PollHostList()__ 检索完整列表。

主服务器的 NAT 穿透任务实际上由一个名为__协调程序 (Facilitator)__ 的独立进程处理，但 Unity 的主服务器会并行运行两个服务。

__游戏类型__是一个标识名称，对于每个游戏都应该是唯一的（虽然 Unity 不提供任何中央注册系统来保证这一点）。因此，应选择一个不太可能被其他人使用的独特名称。如果您的游戏有多个不同版本，可能需要警告用户其客户端与正在运行的服务器版本不兼容。版本信息可传入 comment 字段（这实际上是二进制数据，因此能够以任何所需的形式传入版本）。游戏名称只是由任何人设置的特定游戏实例的名称。

如果对主服务器进行了适当修改，则可以更高级的方式使用 comment 字段（有关如何执行此操作的详细信息，请参阅本页面底部的_高级_部分）。例如，您可以保留 comment 字段的前十个字节用于保存密码，然后在收到主机更新时在主服务器中提取密码。如果密码检查失败，它可以拒绝主机更新。


注册游戏
------------------


在注册游戏之前，必须根据主机是否支持 NAT 功能来启用或禁用该功能；您可以使用 [Network.InitializeServer](../ScriptReference/Network.InitializeServer.html) 的 __useNat__ 参数执行此操作。

可使用如下所示的代码启动服务器：

````
using UnityEngine;
using UnityEngine.Network;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	void OnGUI() {
		if (GUILayout.Button ("Start Server"))
		{
			// 如果不存在公共 IP，应使用 NAT 穿透
			Network.InitializeServer(32, 25002, !Network.HavePublicAddress());
			MasterServer.RegisterHost("MyUniqueGameType", "JohnDoes game", "l33t game for all");
		}
	}
}
````
_C# 脚本示例_

````
function OnGUI() {
	if (GUILayout.Button ("Start Server"))
	{
		// 如果不存在公共 IP，应使用 NAT 穿透
		Network.InitializeServer(32, 25002, !Network.HavePublicAddress());
		MasterServer.RegisterHost("MyUniqueGameType", "JohnDoes game", "l33t game for all");
	}
}
````
_JS 脚本示例_

此处，我们通过检查计算机是否具有公共地址来决定是否需要 NAT 穿透。有一个称为 [Network.TestConnection](../ScriptReference/Network.TestConnection.html) 更复杂函数可以指示主机是否可执行 NAT。该函数还对公共 IP 地址进行连接测试，查看是否有防火墙阻止游戏端口。具有公共 IP 地址的计算机始终能通过 NAT 测试，但如果测试失败，主机将**无法**连接到 NAT 客户端。在这种情况下，应通知用户必须启用端口转发才能使游戏运行。国内宽带连接通常具有 NAT 地址，但无法设置端口转发（因为它们没有个人公共 IP 地址）。在这些情况下，如果 NAT 测试失败，应告知用户运行服务器是不妥的，因为只有同一本地网络上的客户端才能连接。

如果主机启用 NAT 功能但不需要该功能，那么仍然可以访问主机。但是，无法执行 NAT 穿透的客户端可能会错误地认为自己无法在服务器启用 NAT 的基础上进行连接。


连接到游戏
--------------------


在主机注册或查询期间会发送一个 __HostData__ 对象。该对象包含有关主机的以下信息：

| | | |
|:---|:---|:---|
|boolean |__useNat__ |指示主机是否使用 NAT 穿透。 |
|String |__gameType__ |主机的游戏类型。 |
|String |__gameName__ |主机的游戏名称。|
|int |__connectedPlayers__ |当前连接的玩家/客户端数量。|
|int |__playerLimit__ |允许的最多并发玩家/客户端数量。|
|String[] |__IP__ |主机的内部 IP 地址。在具有公共地址的服务器上，外部和内部地址是相同的。此字段定义为数组，因为在内部连接时需要检查与计算机的所有活动接口关联的所有 IP 地址。|
|int |__port__ |主机的接口。|
|boolean |__passwordProtected__ |指示是否需要提供密码才能连接到此主机。|
|String |__comment__ |在主机注册期间设置的任何备注。|
|String |__guid__ |主机的网络 GUID。使用 NAT 穿透技术进行连接时需要此信息。|

客户端可以使用此信息来查看主机的连接功能。启用 NAT 后，您需要在连接时使用主机的 GUID。在连接期间检索 __HostData__ 时，会自动为您处理此信息。连接例程可能如下所示：

````
using UnityEngine;
using UnityEngine.Network;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	void Awake() {
		MasterServer.RequestHostList("MadBubbleSmashGame");
	}
	
	void OnGUI() {
		HostData[] data = MasterServer.PollHostList();
		// 轮训主机列表中的所有主机
		foreach (var element in data)
		{
			GUILayout.BeginHorizontal();	
			var name = element.gameName + " " + element.connectedPlayers + " / " + element.playerLimit;
			GUILayout.Label(name);	
			GUILayout.Space(5);
			string hostInfo;
			hostInfo = "[";
			foreach (var host in element.ip)
				hostInfo = hostInfo + host + ":" + element.port + " ";
			hostInfo = hostInfo + "]";
			GUILayout.Label(hostInfo);	
			GUILayout.Space(5);
			GUILayout.Label(element.comment);
			GUILayout.Space(5);
			GUILayout.FlexibleSpace();
			if (GUILayout.Button("Connect"))
			{
				// 连接到 HostData 结构，在内部使用正确的方法（使用 NAT 时为 GUID）。
				Network.Connect(element);			
			}
			GUILayout.EndHorizontal();	
		}
	}
}
````
_C# 脚本示例_

````
function Awake() {
	MasterServer.RequestHostList("MadBubbleSmashGame");
}

function OnGUI() {
	var data : HostData[] = MasterServer.PollHostList();
	// 访问主机列表中的所有主机
	for (var element in data)
	{
		GUILayout.BeginHorizontal();	
		var name = element.gameName + " " + element.connectedPlayers + " / " + element.playerLimit;
		GUILayout.Label(name);	
		GUILayout.Space(5);
		var hostInfo;
		hostInfo = "[";
		for (var host in element.ip)
			hostInfo = hostInfo + host + ":" + element.port + " ";
		hostInfo = hostInfo + "]";
		GUILayout.Label(hostInfo);	
		GUILayout.Space(5);
		GUILayout.Label(element.comment);
		GUILayout.Space(5);
		GUILayout.FlexibleSpace();
		if (GUILayout.Button("Connect"))
		{
			// 连接到 HostData 结构，在内部使用正确的方法（使用 NAT 时为 GUID）。
			Network.Connect(element);			
		}
		GUILayout.EndHorizontal();	
	}
}
````
_JS 脚本示例_

此示例显示了主服务器返回的所有相关主机信息。在此基础上可添加其他有用的数据（如 ping 信息或主机的地理位置）。

NAT 穿透
----------------


NAT 穿透的可用性可以确定特定计算机是否适合用作服务器。虽然某些客户端可能可以连接，但其他一些客户端可能无法连接到任何 NAT 服务器。

默认情况下，NAT 穿透是在主服务器的帮助下提供的，但不需要这样做。协调程序是实际用于 NAT 穿透服务的进程。如果两台机器连接到协调程序，那么只要使用外部 IP 和端口，它们就会表现为可以相互连接。主服务器用于提供此外部 IP 和端口信息（很难通过其他方式确定此信息）。这就是主服务器和协调程序如此紧密集成的原因。主服务器和协调程序在默认情况下具有相同的 IP 地址，要更改任何一方的地址，请使用 [MasterServer.ipAddress](../ScriptReference/MasterServer-ipAddress.html)、[MasterServer.port](../ScriptReference/MasterServer-port.html)、[Network.natFacilitatorIP](../ScriptReference/Network-natFacilitatorIP.html) 和 [Network.natFacilitatorPort](../ScriptReference/Network-natFacilitatorPort.html)


高级
--------


Unity Technologies 还有一个完全部署的主服务器可用于测试目的，这实际上是默认情况下使用的服务器。但是，源代码可供任何人免费使用，并且服务器可以部署在 Windows、Linux 和 Mac OS 上。除了简单地从源代码构建项目之外，有时还可能需要修改主服务器处理信息的方式及其通信方式。例如，您可以优化主机数据的处理或限制主机列表上返回的客户端数量。此类更改将需要修改源代码；有关如何进行此类操作的信息，请参阅[主服务器构建页面](net-MasterServerBuild.html)。
