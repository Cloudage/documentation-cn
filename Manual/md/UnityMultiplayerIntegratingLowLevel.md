#使用 NetworkTransport 进行集成

如果在集成 Unity Multiplayer 服务时需要最大程度的灵活性，可直接使用 `NetworkTransport` 类。此方法需要更多代码，但可用于控制游戏与 Multiplayer 服务集成的所有细节。

以下示例说明了如何直接使用 `NetworkTransport` 类进行连接：

````
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Networking.Types;
using UnityEngine.Networking.Match;
using System.Collections.Generic;

public class DirectSetup : MonoBehaviour
{
    // 与 Matchmaker 相关
    List<MatchInfoSnapshot> m_MatchList = new List<MatchInfoSnapshot>();
    bool m_MatchCreated;
    bool m_MatchJoined;
    MatchInfo m_MatchInfo;
    string m_MatchName = "NewRoom";
    NetworkMatch m_NetworkMatch;

    // 与连接/通信相关
    int m_HostId = -1;
    // 在服务器上将有多个连接，在客户端上将只包含一个 ID
    List<int> m_ConnectionIds = new List<int>();
    byte[] m_ReceiveBuffer;
    string m_NetworkMessage = "Hello world";
    string m_LastReceivedMessage = "";
    NetworkWriter m_Writer;
    NetworkReader m_Reader;
    bool m_ConnectionEstablished;

    const int k_ServerPort = 25000;
    const int k_MaxMessageSize = 65535;

    void Awake()
    {
        m_NetworkMatch = gameObject.AddComponent<NetworkMatch>();
    }

    void Start()
    {
        m_ReceiveBuffer = new byte[k_MaxMessageSize];
        m_Writer = new NetworkWriter();
        // 在一台机器上使用多个独立玩家进行测试时，需要启用此功能
        Application.runInBackground = true;
    }

    void OnApplicationQuit()
    {
        NetworkTransport.Shutdown();
    }

    void OnGUI()
    {
        if (string.IsNullOrEmpty(Application.cloudProjectId))
            GUILayout.Label("You must set up the project first.See the Multiplayer tab in the Service Window");
        else
            GUILayout.Label("Cloud Project ID: " + Application.cloudProjectId);

        if (m_MatchJoined)
            GUILayout.Label("Match joined '" + m_MatchName + "' on Matchmaker server");
        else if (m_MatchCreated)
            GUILayout.Label("Match '" + m_MatchName + "' created on Matchmaker server");

        GUILayout.Label("Connection Established: " + m_ConnectionEstablished);

        if (m_MatchCreated || m_MatchJoined)
        {
            GUILayout.Label("Relay Server: " + m_MatchInfo.address + ":" + m_MatchInfo.port);
            GUILayout.Label("NetworkID: " + m_MatchInfo.networkId + " NodeID: " + m_MatchInfo.nodeId);
            GUILayout.BeginHorizontal();
            GUILayout.Label("Outgoing message:");
            m_NetworkMessage = GUILayout.TextField(m_NetworkMessage);
            GUILayout.EndHorizontal();
            GUILayout.Label("Last incoming message: " + m_LastReceivedMessage);

            if (m_ConnectionEstablished && GUILayout.Button("Send message"))
            {
                m_Writer.SeekZero();
                m_Writer.Write(m_NetworkMessage);
                byte error;
                for (int i = 0; i < m_ConnectionIds.Count; ++i)
                {
                    NetworkTransport.Send(m_HostId,
                        m_ConnectionIds[i], 0, m_Writer.AsArray(), m_Writer.Position, out error);
                    if ((NetworkError)error != NetworkError.Ok)
                        Debug.LogError("Failed to send message: " + (NetworkError)error);
                }
            }

            if (GUILayout.Button("Shutdown"))
            {
                m_NetworkMatch.DropConnection(m_MatchInfo.networkId,
                    m_MatchInfo.nodeId, 0, OnConnectionDropped);
            }
        }
        else
        {
            if (GUILayout.Button("Create Room"))
            {
                m_NetworkMatch.CreateMatch(m_MatchName, 4, true, "", "", "", 0, 0, OnMatchCreate);
            }

            if (GUILayout.Button("Join first found match"))
            {
                m_NetworkMatch.ListMatches(0, 1, "", true, 0, 0, (success, info, matches) =>
                {
                    if (success && matches.Count > 0)
                        m_NetworkMatch.JoinMatch(matches[0].networkId, "", "", "", 0, 0, OnMatchJoined);
                });
            }

            if (GUILayout.Button("List rooms"))
            {
                m_NetworkMatch.ListMatches(0, 20, "", true, 0, 0, OnMatchList);
            }

            if (m_MatchList.Count > 0)
            {
                GUILayout.Label("Current rooms:");
            }
            foreach (var match in m_MatchList)
            {
                if (GUILayout.Button(match.name))
                {
                    m_NetworkMatch.JoinMatch(match.networkId, "", "", "", 0, 0, OnMatchJoined);
                }
            }
        }
    }

    public void OnConnectionDropped(bool success, string extendedInfo)
    {
        Debug.Log("Connection has been dropped on matchmaker server");
        NetworkTransport.Shutdown();
        m_HostId = -1;
        m_ConnectionIds.Clear();
        m_MatchInfo = null;
        m_MatchCreated = false;
        m_MatchJoined = false;
        m_ConnectionEstablished = false;
    }

    public virtual void OnMatchCreate(bool success, string extendedInfo, MatchInfo matchInfo)
    {
        if (success)
        {
            Debug.Log("Create match succeeded");
            Utility.SetAccessTokenForNetwork(matchInfo.networkId, matchInfo.accessToken);
           
            m_MatchCreated = true;
            m_MatchInfo = matchInfo;

            StartServer(matchInfo.address, matchInfo.port, matchInfo.networkId,
                matchInfo.nodeId);
        }
        else
        {
            Debug.LogError("Create match failed: " + extendedInfo);
        }
    }

    public void OnMatchList(bool success, string extendedInfo, List<MatchInfoSnapshot> matches)
    {
        if (success && matches != null)
        {
            m_MatchList = matches;
        }
    else if(!success)
    {
        Debug.LogError("List match failed: " + extendedInfo);
    }
    }

    // 当我们配对后，我们连接到服务器/主机
    public virtual void OnMatchJoined(bool success, string extendedInfo, MatchInfo matchInfo)
    {
        if (success)
        {
            Debug.Log("Join match succeeded");
            Utility.SetAccessTokenForNetwork(matchInfo.networkId, matchInfo.accessToken);

            m_MatchJoined = true;
            m_MatchInfo = matchInfo;

            Debug.Log("Connecting to Address:" + matchInfo.address +
                " Port:" + matchInfo.port +
                " NetworKID: " + matchInfo.networkId +
                " NodeID: " + matchInfo.nodeId);
            ConnectThroughRelay(matchInfo.address, matchInfo.port, matchInfo.networkId,
                matchInfo.nodeId);
        }
        else
        {
            Debug.LogError("Join match failed: " + extendedInfo);
        }
    }

    void SetupHost(bool isServer)
    {
        Debug.Log("Initializing network transport");
        NetworkTransport.Init();
        var config = new ConnectionConfig();
        config.AddChannel(QosType.Reliable);
        config.AddChannel(QosType.Unreliable);
        var topology = new HostTopology(config, 4);
        if (isServer)
            m_HostId = NetworkTransport.AddHost(topology, k_ServerPort);
        else
            m_HostId = NetworkTransport.AddHost(topology);
    }

    void StartServer(string relayIp, int relayPort, NetworkID networkId, NodeID nodeId)
    {
        SetupHost(true);

        byte error;
        NetworkTransport.ConnectAsNetworkHost(
            m_HostId, relayIp, relayPort, networkId, Utility.GetSourceID(), nodeId, out error);
    }

    void ConnectThroughRelay(string relayIp, int relayPort, NetworkID networkId, NodeID nodeId)
    {
        SetupHost(false);

        byte error;
        NetworkTransport.ConnectToNetworkPeer(
            m_HostId, relayIp, relayPort, 0, 0, networkId, Utility.GetSourceID(), nodeId, out error);
    }

    void Update()
    {
        if (m_HostId == -1)
            return;

        var networkEvent = NetworkEventType.Nothing;
        int connectionId;
        int channelId;
        int receivedSize;
        byte error;

        // 从中继连接中获取事件
        networkEvent = NetworkTransport.ReceiveRelayEventFromHost(m_HostId, out error);
        if (networkEvent == NetworkEventType.ConnectEvent)
            Debug.Log("Relay server connected");
        if (networkEvent == NetworkEventType.DisconnectEvent)
            Debug.Log("Relay server disconnected");

        do
        {
            // 从服务器/客户端游戏连接中获取事件
            networkEvent = NetworkTransport.ReceiveFromHost(m_HostId, out connectionId, out channelId,
                m_ReceiveBuffer, (int)m_ReceiveBuffer.Length, out receivedSize, out error);
            if ((NetworkError)error != NetworkError.Ok)
            {
                Debug.LogError("Error while receiveing network message: " + (NetworkError)error);
            }

            switch (networkEvent)
            {
                case NetworkEventType.ConnectEvent:
                    {
                        Debug.Log("Connected through relay, ConnectionID:" + connectionId +
                            " ChannelID:" + channelId);
                        m_ConnectionEstablished = true;
                        m_ConnectionIds.Add(connectionId);
                        break;
                    }
                case NetworkEventType.DataEvent:
                    {
                        Debug.Log("Data event, ConnectionID:" + connectionId +
                            " ChannelID: " + channelId +
                            " Received Size: " + receivedSize);
                        m_Reader = new NetworkReader(m_ReceiveBuffer);
                        m_LastReceivedMessage = m_Reader.ReadString();
                        break;
                    }
                case NetworkEventType.DisconnectEvent:
                    {
                        Debug.Log("Connection disconnected, ConnectionID:" + connectionId);
                        break;
                    }
                case NetworkEventType.Nothing:
                    break;
            }
        } while (networkEvent != NetworkEventType.Nothing);
    }
}
````
