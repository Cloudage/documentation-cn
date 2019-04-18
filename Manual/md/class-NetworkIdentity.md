# Network Identity

Network Identity 组件是 Unity 网络高级 API 的核心。该组件可控制游戏对象在网络上的唯一身份，并使用该身份使网络系统识别游戏对象。

![Inspector 窗口中的 Network Identity 组件](../uploads/Main/NetworkIdentityScreenshot.png)

|**属性**|**功能**|
|:---|:---|
|Server Only|勾选此复选框可确保 Unity 仅在服务器上生成游戏对象，而不在客户端上生成游戏对象。|
|Local Player Authority|勾选此复选框可将此游戏对象的授权网络控制权提供给拥有此游戏对象的客户端。该客户端上的玩家游戏对象拥有针对此游戏对象的授权。其他组件（如 Network Transform）使用此属性来确定将哪个客户端视为授权来源。|


## 实例化的网络游戏对象

对于 Unity 的服务器授权网络系统，服务器必须使用 [NetworkServer.Spawn](../ScriptReference/Networking.NetworkServer.Spawn.html) 生成具有网络身份的联网游戏对象。这样会自动在连接到服务器的客户端上创建这些游戏对象，并为它们分配 [NetworkInstanceId](../ScriptReference/Networking.NetworkInstanceId.html)。

必须在运行时生成的所有预制件上添加 Network Identity 组件，才能让网络系统使用这些预制件。请参阅[对象生成](UNetSpawning.html)以了解更多信息。

## 基于场景的网络游戏对象

还可以将保存为场景一部分的游戏对象（例如，环境道具）联网。网络游戏对象使这些游戏对象的行为略有不同，因为需要在网络上生成这些游戏对象。

在构建游戏时，Unity 会禁用包含 Network Identity 组件的所有基于场景的游戏对象。当客户端连接到服务器时，服务器发送生成消息以告知客户端启用哪些场景游戏对象以及这些场景游戏对象的最新状态信息。这样可以确保，当客户端开始游戏时，游戏不会在错误位置包含游戏对象，或者 Unity 不会在连接时生成并立即销毁游戏对象（例如，如果一个事件在该客户端连接之前删除了游戏对象）。请参阅[联网场景游戏对象](UNetSceneObjects.html)以了解更多信息。

## 预览面板中的 Network Information 部分

此组件包含网络跟踪信息，并在预览面板中显示该信息。例如，已为对象分配的场景 ID、网络 ID 和资源 ID。因此可以检查可用于进行调查和调试的信息。

场景 ID 在具有 NetworkIdentity 组件的所有场景对象中都有效。网络 ID 是此特定对象实例的 ID。可能存在从特定预制件进行实例化的多个对象，而网络 ID 可用于标识应该应用网络更新的对象。资源 ID 表示实例化对象的源资源。在网络上生成特定的游戏对象预制件时会在内部使用资源 ID。

![](../uploads/Main/NetworkIdentityPreview.png) 

在运行时，此处将显示更多信息（禁用的 NetworkBehaviour 以非粗体显示）：

![](../uploads/Main/NetworkIdentityPreviewRuntime.png) 

## 预览面板信息

|属性|描述|
|:---|:---|
|assetId|标识与此对象关联的预制件（用于生成）。|
|clientAuthorityOwner|对该对象拥有授权的客户端。如果没有客户端拥有授权，则此属性为 null。|
|connectionToClient|与此 NetworkIdentity 关联的 NetworkConnection。此属性只对服务器上的玩家对象有效。|
|connectionToServer|与此 NetworkIdentity 关联的 NetworkConnection。此属性只对本地客户端上的玩家对象有效。|
|hasAuthority|如果此对象是对象的授权版本，则为 true。这将意味着是服务器上的普通对象，或者在客户端上具有 localPlayerAuthority 的对象。|
|isClient|如果此对象在客户端上运行，则为 true。|
|isLocalPlayer|如果此对象代表本地计算机上的玩家，则返回 true。|
|isServer|如果此对象在服务器上运行并且已生成，则为 true。|
|localPlayerAuthority|如果此对象由拥有此对象的客户端控制，则为 true - 该客户端上的本地玩家对象具有针对此对象的授权。由 NetworkTransform 等其他组件使用。|
|netId|此网络会话的唯一标识符，在生成时分配。|
|observers|能够看到此对象的客户端 NetworkConnection 列表。此属性为只读。|
|playerControllerId|与此对象关联的控制器的标识符。仅对玩家对象有效。|
|SceneId|场景中的联网对象的唯一标识符。此属性仅在播放模式下填充。|
|serverOnly|此标志用于指示不在客户端上生成该对象。|


