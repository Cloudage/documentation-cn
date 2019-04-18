##自定义的配对回调

将 [Network Manager](class-NetworkManager.html) 与 [Network Manager HUD](class-NetworkManagerHUD.html) 结合使用时，[NetworkManager.StartMatchmaker](../ScriptReference/Networking.NetworkManager.StartMatchMaker.html) 方法支持进行配对，并使用 [NetworkMatch](../ScriptReference/Networking.Match.NetworkMatch.html) 对象来填充 [NetworkManager.matchMaker](../ScriptReference/Networking.NetworkManager-matchMaker.html) 属性。在激活此属性后，Network Manager HUD 将使用此属性并调用 [NetworkManager](../ScriptReference/Networking.NetworkManager.html) 上的方法，让您执行简单配对。

`NetworkManager` 上有一些虚拟函数，您可以通过从 `NetworkManager` 派生自己的类来自定义这些虚拟函数。然后，可以自定义新的 `NetworkManager` 类对 Matchmaker 回调的响应方式。

下面显示了这些回调及其默认实现。如果重写这些回调，有些方法会要求您调用基本实现，否则 Network Manager HUD 的功能将中断。例如，`OnMatchCreate` 的默认实现（用于启动主机）。

```
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Networking.Match;

public class CustomManager : NetworkManager {

    public override void OnMatchCreate(bool success, string extendedInfo, MatchInfo matchInfo) {
        if (LogFilter.logDebug) { Debug.LogFormat("NetworkManager OnMatchCreate Success:{0}, ExtendedInfo:{1}, matchInfo:{2}", success, extendedInfo, matchInfo); }
        if(success)
            StartHost(matchInfo);
    }

    public override void OnMatchJoined(bool success, string extendedInfo, MatchInfo matchInfo) {
        if (LogFilter.logDebug) { Debug.LogFormat("NetworkManager OnMatchJoined Success:{0}, ExtendedInfo:{1}, matchInfo:{2}", success, extendedInfo, matchInfo); }
        if(success)
            StartClient(matchInfo);
    }

    public override void OnMatchList(bool success, string extendedInfo, List<MatchInfoSnapshot> matchList) {
        if (LogFilter.logDebug) { Debug.LogFormat("NetworkManager OnMatchList Success:{0}, ExtendedInfo:{1}, matchList.Count:{2}", success, extendedInfo, matchList.Count); }
        matches = matchList;
    }

    public override void OnDestroyMatch(bool success, string extendedInfo) {
        if (LogFilter.logDebug) { Debug.LogFormat("NetworkManager OnDestroyMatch Success:{0}, ExtendedInfo:{1}", success, extendedInfo); }
    }

    public override void OnDropConnection(bool success, string extendedInfo) {
        if (LogFilter.logDebug) { Debug.LogFormat("NetworkManager OnDestroyMatch Success:{0}, ExtendedInfo:{1}", success, extendedInfo); }
    }

    public override void OnSetMatchAttributes(bool success, string extendedInfo) {
        if (LogFilter.logDebug) { Debug.LogFormat("NetworkManager OnDestroyMatch Success:{0}, ExtendedInfo:{1}", success, extendedInfo); }
    }
}
```
