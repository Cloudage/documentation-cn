网络加载关卡（旧版）
============================

*（对于新项目，应使用 [5.1 版中引入的新网络系统](UNet.html)。此信息适用于使用旧网络系统的旧版项目。）*

下面是在多人游戏中加载关卡的方法的一个简单示例。此示例确保在加载关卡时不处理任何网络消息。此外，还确保在一切准备就绪之前不会发送任何消息。最后，加载关卡时，将向所有脚本发送一条消息，以便它们知道关卡已加载并可以对此作出反应。__SetLevelPrefix__ 函数有助于在新加载的关卡的范围内阻止不需要的网络更新。例如，不需要的更新可能是上一关卡的更新。此示例还使用组将游戏数据和关卡加载通信分成组。组 0 用于游戏数据流量，组 1 用于关卡加载。加载关卡时将阻止组 0，但组 1 保持开放状态，此外还可以中继聊天通信，以便在关卡加载期间保持开放状态。

````
using UnityEngine;
using UnityEngine.Network;
using System.Collections;

[RequireComponent(NetworkView)]
public class ExampleScript : MonoBehaviour {
	string[] supportedNetworkLevels  = new[]{ "mylevel" };
	string disconnectedLevel = "loader";
	int lastLevelPrefix = 0;
	NetworkView networkView;	

	void Awake ()
	{
		// 网络加载关卡在单独的通道中完成。
		DontDestroyOnLoad(this);
		networkView = new NetworkView ();
		networkView.group = 1;
		Application.LoadLevel(disconnectedLevel);
	}
	
	void OnGUI ()
	{
		if (Network.peerType != NetworkPeerType.Disconnected)
		{
			GUILayout.BeginArea(Rect(0, Screen.height - 30, Screen.width, 30));
			GUILayout.BeginHorizontal();
			
			foreach (var level in supportedNetworkLevels)
			{
				if (GUILayout.Button(level))
				{
					Network.RemoveRPCsInGroup(0);
					Network.RemoveRPCsInGroup(1);
					networkView.RPC("LoadLevel", RPCMode.AllBuffered, level, lastLevelPrefix + 1);
				}
			}
			GUILayout.FlexibleSpace();
			GUILayout.EndHorizontal();
			GUILayout.EndArea();
		}
	}
	
	[RPC]
	IEnumerator LoadLevel (string level, int levelPrefix)
	{
		lastLevelPrefix = levelPrefix;
		
		// 没有理由在默认通道上通过网络发送更多数据，
		// 因为我们即将加载关卡，所以所有这些对象都会被删除
		Network.SetSendingEnabled(0, false);	
		
		// 我们需要停止接收，因为首先必须加载关卡。
		//加载关卡后，允许触发附加到关卡中对象的 RPC 和其他状态更新
		Network.isMessageQueueRunning = false;
		
		// 从关卡加载的所有网络视图都将在其 NetworkViewID 中加入前缀。
		//这将阻止客户端的旧更新泄漏到新创建的场景中。
		Network.SetLevelPrefix(levelPrefix);
		Application.LoadLevel(level);
		yield return;

		//允许再次接收数据
		Network.isMessageQueueRunning = true;
		// 现在已经加载该关卡，我们可以开始向客户端发送数据
		Network.SetSendingEnabled(0, true);

		var gameObjects = FindObjectsOfType(GameObject) as GameObject[];
		foreach (var go in gameObjects)
			go.SendMessage("OnNetworkLoadedLevel", SendMessageOptions.DontRequireReceiver);	
	}
	
	void OnDisconnectedFromServer ()
	{
		Application.LoadLevel(disconnectedLevel);
	}
}
````
_C# 示例脚本_

````
var supportedNetworkLevels : String[] = [ "mylevel" ];
var disconnectedLevel : String = "loader";
private var lastLevelPrefix = 0;

function Awake ()
{
    // 网络加载关卡在单独的通道中完成。
    DontDestroyOnLoad(this);
    networkView.group = 1;
    Application.LoadLevel(disconnectedLevel);
}

function OnGUI ()
{
	if (Network.peerType != NetworkPeerType.Disconnected)
	{
		GUILayout.BeginArea(Rect(0, Screen.height - 30, Screen.width, 30));
		GUILayout.BeginHorizontal();
		
		for (var level in supportedNetworkLevels)
		{
			if (GUILayout.Button(level))
			{
				Network.RemoveRPCsInGroup(0);
				Network.RemoveRPCsInGroup(1);
				networkView.RPC("LoadLevel", RPCMode.AllBuffered, level, lastLevelPrefix + 1);
			}
		}
		GUILayout.FlexibleSpace();
		GUILayout.EndHorizontal();
		GUILayout.EndArea();
	}
}

@RPC
function LoadLevel (level : String, levelPrefix : int)
{
	lastLevelPrefix = levelPrefix;

		// 没有理由在默认通道上通过网络发送更多数据，
		// 因为我们即将加载关卡，所以所有这些对象都会被删除
		Network.SetSendingEnabled(0, false);	

		// 我们需要停止接收，因为首先必须加载关卡。
		//加载关卡后，允许触发附加到关卡中对象的 RPC 和其他状态更新
		Network.isMessageQueueRunning = false;
		
		// 从关卡加载的所有网络视图都将在其 NetworkViewID 中加入前缀。
		//这将阻止客户端的旧更新泄漏到新创建的场景中。
		Network.SetLevelPrefix(levelPrefix);
		Application.LoadLevel(level);
		yield;

		//允许再次接收数据
		Network.isMessageQueueRunning = true;
		// 现在已经加载该关卡，我们可以开始向客户端发送数据
		Network.SetSendingEnabled(0, true);

		for (var go in FindObjectsOfType(GameObject))
			go.SendMessage("OnNetworkLoadedLevel", SendMessageOptions.DontRequireReceiver);	
}

function OnDisconnectedFromServer ()
{
	Application.LoadLevel(disconnectedLevel);
}

@script RequireComponent(NetworkView)
````
_JS 示例脚本_
