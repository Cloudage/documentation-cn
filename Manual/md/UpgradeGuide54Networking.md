#5.4 网络 API 更改

在 Unity 5.4 中，我们对配对 API 进行了一些更改。我们的目的是简化和清理 API。

如果在早期版本的 Unity 中使用了配对 API，则需要检查并更新下面列出的类和函数。

__MatchDesc__ 已重命名为 __MatchInfoSnapshot__。

删除了所有请求和响应类，因此 NetworkMatch 中不再有任何重载函数。相反，我们更新了所有函数的参数列表以适应缺少类的问题，并更新了 2 个委托。



###设置

````
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Networking.Match;
NetworkMatch matchMaker;
void Awake()
{
    matchMaker = gameObject.AddComponent<NetworkMatch>();
} 
````

### CreateMatch（5.4 版之前）

````
CreateMatchRequest create = new CreateMatchRequest();
...
matchMaker.CreateMatch(create, OnMatchCreate);
````

或者

````
matchMaker.CreateMatch("roomName", 4, true, "", OnMatchCreate);
````

现在改为：

````
matchMaker.CreateMatch("roomName", 4, true, "", "", "", 0, 0, OnMatchCreate);
````


###CreateMatch 回调（5.4 版之前）

````
public void OnMatchCreate(CreateMatchResponse matchResponse)
{
    ...
} 
````

现在改为：

````
public void OnMatchCreate(bool success, string extendedInfo, MatchInfo matchInfo)
{
    ...
} 
````

###ListMatches（5.4 版之前）

````
ListMatchRequest list = new ListMatchRequest();

matchMaker.ListMatches(list, OnMatchList);
````

或者

````
matchMaker.ListMatches(0, 10, "", OnMatchList);
````

现在改为：

````
matchMaker.ListMatches(0, 10, "", true, 0, 0, OnMatchList);
````

###ListMatches 回调（5.4 版之前）

````
public void OnMatchList(ListMatchResponse matchListResponse)
{
    ...
} 
````

现在改为：

````
public void OnMatchList(bool success, string extendedInfo, List<MatchInfoSnapshot> matches)
{
    ...
} 
````


###JoinMatch（5.4 版之前）

````
JoinMatchRequest join = new JoinMatchRequest();

matchMaker.JoinMatch(join, OnMatchJoined);
````

或者

````
matchMaker.JoinMatch(match.networkId, "", OnMatchJoined);
````

现在改为：

````
matchMaker.JoinMatch(networkId, "" , "", "", 0, 0, OnMatchJoined);
````

###JoinMatch 回调（5.4 版之前）

````
public void OnMatchJoined(JoinMatchResponse matchJoin)
{
    ...
} 
````

现在改为：

````
public void OnMatchJoined(bool success, string extendedInfo, MatchInfo matchInfo)
{
    ...
} 
````

###DestroyMatch（5.4 版之前）

````
DestroyMatchRequest destroy = DestroyMatchRequest();

matchMaker.DestroyMatch(destroy, OnMatchDestroy);
````

或者

````
matchMaker.DestroyMatch(netId, OnDestroy);
````

现在改为：

````
matchMaker.DestroyMatch(netId, 0, OnMatchDestroy);
````

###DestroyMatch 回调（5.4 版之前）

````
public void OnMatchDestroy(BasicResponse response)
{
    ...
} 
````

现在改为：

````
public void OnMatchDestroy(bool success, string extendedInfo)
{
    ...
} 
````

###DropConnection（5.4 版之前）

````
DropConnectionRequest drop = DropConnectionRequest();

matchMaker.DropConnection(drop, OnMatchDropConnection);
````

或者

````
matchMaker.DropConnection(netId, nodeId, OnMatchDropConnection);
````

现在改为：

````
matchMaker.DropConnection(netId, nodeId, 0, OnMatchDropConnection);
````

###DropConnection 回调（5.4 版之前）

````
public void OnMatchDropConnection(BasicResponse response)
{
    ...
} 
````

现在改为：

````
public void OnMatchDropConnection(bool success, string extendedInfo)
{
    ...
} 
````
