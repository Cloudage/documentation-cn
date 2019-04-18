将单人游戏转换为 Unity Multiplayer 多人游戏
==============================================

本文档将介绍使用新的 Unity Multiplayer 网络系统将单人游戏转换为多人游戏的步骤。本文描述的过程是真实游戏实际过程的简化浓缩版本；实际情况并不完全与此相同，但提供了该过程的基本方案。

NetworkManager 设置
--------------------

* 在场景中添加一个新的游戏对象，然后将其重命名为“NetworkManager”。
* 在“NetworkManager”游戏对象中添加 __NetworkManager__ 组件。
* 在游戏对象中添加 __NetworkManagerHUD__ 组件。这样可提供用于管理网络游戏状态的默认 UI。

请参阅[使用 NetworkManager](UNetManager.html)。

玩家预制件设置
-------------------

* 找到游戏中玩家游戏对象的预制件，或者基于玩家游戏对象创建预制件
* 在玩家预制件中添加 __NetworkIdentity__ 组件
* 在 NetworkIdentity 上选中 LocalPlayerAuthority 复选框
* 将 NetworkManager 的 __Spawn Info__ 部分中的 `playerPrefab` 设置为玩家预制件
* 从场景中删除玩家游戏对象实例（如果在场景中存在）

请参阅[玩家对象](UNetPlayers.html)以了解更多信息。

玩家移动
---------------

* 在玩家预制件中添加 NetworkTransform 组件
* 更新输入和控制脚本以采用 `isLocalPlayer`
* 修正摄像机以使用生成的玩家和 `isLocalPlayer`

例如，以下脚本仅处理本地玩家的输入：

````
using UnityEngine;
using UnityEngine.Networking;

public class Controls : NetworkBehaviour
{
	void Update()
	{
		if (!isLocalPlayer)
		{
			// 如果不是本地玩家，则退出更新
			return;
		}

		// 处理玩家的移动输入
	}
}
````

玩家的基本游戏状态
-----------------------

* 将包含重要数据的脚本放入 NetworkBehaviour 而不是 MonoBehaviour
* 将重要的成员变量放入 SyncVar

请参阅[状态同步](UNetStateSync.html)。


网络化操作
-----------------

* 将执行重要操作的脚本放入 NetworkBehaviour 而不是 MonoBehaviour
* 将执行玩家重要操作的函数更新为命令

请参阅[网络化操作](UNetActions.html)。


非玩家游戏对象
------------------
修正非玩家预制件，如敌人：

* 添加 __NetworkIdentify__ 组件
* 添加 __NetworkTransform__ 组件
* 在 __NetworkManager__ 中注册可生成的预制件
* 用游戏状态和操作来更新脚本


生成器
--------

* 可将生成器脚本更改为 NetworkBehaviour
* 修改生成器以便仅在服务器上运行（使用 isServer 属性或 `OnStartServer()` 函数）
* 为创建的游戏对象调用 `NetworkServer.Spawn()`

玩家生成位置
---------------------------

* 添加一个新的游戏对象并将其放置在玩家的起始位置
* 在新游戏对象中添加 __NetworkStartPosition__ 组件

大厅
-----

* 创建大厅场景
* 在场景中添加一个新的游戏对象，然后将其重命名为“NetworkLobbyManager”。
* 在新游戏对象中添加 NetworkLobbyManager 组件。
* 配置管理器：
    * 场景
    * 预制件
    * 生成器



