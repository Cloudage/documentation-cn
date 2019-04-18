#网络客户端和服务器

许多的多人游戏都可以使用 [Network Manager](class-NetworkManager.html) 来管理连接，但您也可以直接使用较低级别的 [NetworkServer](../ScriptReference/Networking.NetworkServer.html) 和 [NetworkClient](../ScriptReference/Networking.NetworkClient.html) 类。

使用[高级 API](UNetUsingHLAPI.html) 时，每个游戏都必须有一台可供连接的主机服务器。多人游戏中的每个参与者可以是客户端、专用服务器或同时是服务器与客户端的组合。对于没有专用服务器的多人游戏，这种组合角色是常见情况。

对于没有专用服务器的多人游戏，运行游戏的其中一个玩家充当该游戏的服务器。该玩家的游戏实例运行“本地客户端”，而不是普通的远程客户端。本地客户端使用与服务器相同的 Unity 场景和游戏对象，并使用消息队列进行内部通信，而不是通过网络发送消息。对于 HLAPI 代码和系统而言，本地客户端只是另一个客户端，因此几乎所有用户代码都是相同的，无论客户端是本地客户端还是远程客户端。因此，利用相同代码制作同时支持多人游戏和单机模式的游戏将变得容易。

多人游戏的常见模式是设定一个管理游戏网络状态的游戏对象。下面是一个 NetworkManager 脚本的开头部分。此脚本将附加到游戏启动场景中的游戏对象。它具有简单的 UI 和键盘处理功能，允许游戏以不同的网络模式启动。在发布游戏之前，应创建一个更具视觉吸引力的菜单，在其中添加“Start single player game”和“Start multiplayer game”等选项。

```

using UnityEngine;
using UnityEngine.Networking;

public class MyNetworkManager : MonoBehaviour {
    public bool isAtStartup = true;
    NetworkClient myClient;
    void Update () 
    {
        if (isAtStartup)
        {
            if (Input.GetKeyDown(KeyCode.S))
            {
                SetupServer();
            }
            if (Input.GetKeyDown(KeyCode.C))
            {
                SetupClient();
            }
            if (Input.GetKeyDown(KeyCode.B))
            {
                SetupServer();
                SetupLocalClient();
            }
        }
    }
    void OnGUI()
    {
        if (isAtStartup)
        {
            GUI.Label(new Rect(2, 10, 150, 100), "Press S for server");     
            GUI.Label(new Rect(2, 30, 150, 100), "Press B for both");       
            GUI.Label(new Rect(2, 50, 150, 100), "Press C for client");
        }
    }   
}

```

这段基本代码调用设置函数来实现各种功能。以下是每种情况的简单设置函数。这些函数为每种情况创建服务器或正确类型的客户端。请注意，远程客户端假定服务器位于同一台机器 (127.0.0.1) 上。对于已完成的游戏，这将是互联网地址，或由[配对系统](UnityMultiplayerSettingUp.html)提供的地址。

```

// 创建服务器并监听端口
    public void SetupServer()
    {
        NetworkServer.Listen(4444);
        isAtStartup = false;
    }
    
    // 创建客户端并连接到服务器端口
    public void SetupClient()
    {
        myClient = new NetworkClient();
        myClient.RegisterHandler(MsgType.Connect, OnConnected);     
        myClient.Connect("127.0.0.1", 4444);
        isAtStartup = false;
    }
    
    // 创建本地客户端并连接到本地服务器
    public void SetupLocalClient()
    {
        myClient = ClientScene.ConnectLocalServer();
        myClient.RegisterHandler(MsgType.Connect, OnConnected);     
        isAtStartup = false;
    }
```

此代码中的客户端为连接事件 [MsgType.Connect](../ScriptReference/Networking.MsgType.Connect.html) 注册回调函数。这是 HLAPI 的内置消息，当客户端连接到服务器时，脚本会调用该消息。在本示例中，客户端上的处理程序的代码为：

```

// 客户端函数
    public void OnConnected(NetworkMessage netMsg)
    {
        Debug.Log("Connected to server");
    }
```

这足以让多人游戏应用程序正常运行。通过此脚本，可使用 [NetworkClient.Send](../ScriptReference/Networking.NetworkClient.Send.html) 和 [NetworkServer.SendToAll](../ScriptReference/Networking.NetworkServer.SendToAll.html) 发送网络消息。请注意，发送消息是与系统交互的低级方式。
