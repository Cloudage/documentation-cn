#使用 Unity 的高级 API 进行集成

要使用[网络高级 API](UNetUsingHLAPI.html) 来集成 Unity Multiplayer 服务，必须直接在脚本中使用 [NetworkMatch](../ScriptReference/Networking.Match.NetworkMatch.html) 类。要使用此类，必须在 `NetworkMatch` 中手动调用函数并自己处理回调。

以下示例说明了如何仅使用 [NetworkMatch](../ScriptReference/Networking.Match.NetworkMatch.html)、[NetworkServer](../ScriptReference/Networking.NetworkServer.html) 和 [NetworkClient](../ScriptReference/Networking.NetworkClient.html) 类来创建比赛、列出比赛和加入比赛。

此脚本将 Matchmaker 设置为指向公共的 Unity Matchmaker 服务器。此脚本将调用 `NetworkMatch` 函数来创建、列出和加入比赛：

* [CreateMatch](../ScriptReference/Networking.Match.NetworkMatch.CreateMatch.html) 可创建比赛
* [JoinMatch](../ScriptReference/Networking.Match.NetworkMatch.JoinMatch.html) 可加入比赛
* [ListMatches](../ScriptReference/Networking.Match.NetworkMatch.ListMatches.html) 可列出 Matchmaker 服务器上注册的比赛

在内部，`NetworkMatch` 使用 Web 服务来建立比赛，并在处理完成时调用给定的回调函数，比如调用 `OnMatchCreate` 来创建比赛。

````
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Networking.Match;
using System.Collections.Generic;

public class HostGame : MonoBehaviour
{
    List<MatchInfoSnapshot> matchList = new List<MatchInfoSnapshot>();
    bool matchCreated;
    NetworkMatch networkMatch;

    void Awake()
    {
        networkMatch = gameObject.AddComponent<NetworkMatch>();
    }

    void OnGUI()
    {
        // 通常不会加入自己创建的比赛，但此处为了演示目的允许这样做。
        if (GUILayout.Button("Create Room"))
        {
            string matchName = "room";
            uint matchSize = 4;
            bool matchAdvertise = true;
            string matchPassword = "";

            networkMatch.CreateMatch(matchName, matchSize, matchAdvertise, matchPassword, "", "", 0, 0, OnMatchCreate);
        }

        if (GUILayout.Button("List rooms"))
        {
            networkMatch.ListMatches(0, 20, "", true, 0, 0, OnMatchList);
        }

        if (matchList.Count > 0)
        {
            GUILayout.Label("Current rooms");
        }
        foreach (var match in matchList)
        {
            if (GUILayout.Button(match.name))
            {
                networkMatch.JoinMatch(match.networkId, "", "", "", 0, 0, OnMatchJoined);
            }
        }
    }

    public void OnMatchCreate(bool success, string extendedInfo, MatchInfo matchInfo)
    {
        if (success)
        {
            Debug.Log("Create match succeeded");
            matchCreated = true;
            NetworkServer.Listen(matchInfo, 9000);
            Utility.SetAccessTokenForNetwork(matchInfo.networkId, matchInfo.accessToken);
        }
        else
        {
            Debug.LogError("Create match failed: " + extendedInfo);
        }
    }

    public void OnMatchList(bool success, string extendedInfo, List<MatchInfoSnapshot> matches)
    {
        if (success && matches != null && matches.Count > 0)
        {
            networkMatch.JoinMatch(matches[0].networkId, "", "", "", 0, 0, OnMatchJoined);
        }
        else if (!success)
        {
            Debug.LogError("List match failed: " + extendedInfo);
        }
    }

    public void OnMatchJoined(bool success, string extendedInfo, MatchInfo matchInfo)
    {
        if (success)
        {
            Debug.Log("Join match succeeded");
            if (matchCreated)
            {
                Debug.LogWarning("Match already set up, aborting...");
                return;
            }
            Utility.SetAccessTokenForNetwork(matchInfo.networkId, matchInfo.accessToken);
            NetworkClient myClient = new NetworkClient();
            myClient.RegisterHandler(MsgType.Connect, OnConnected);
            myClient.Connect(matchInfo);
        }
        else
        {
            Debug.LogError("Join match failed " + extendedInfo);
        }
    }

    public void OnConnected(NetworkMessage msg)
    {
        Debug.Log("Connected!");
    }
}
````
