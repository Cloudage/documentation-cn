#Network Discovery

Network Discovery 组件允许 Unity 应用程序使用网络系统在局域网 (LAN) 上发现彼此。此组件不能用于互联网游戏发现。应使用 Multiplayer 服务（以及 MatchMaker 和 Relay 服务）进行连接。

Network Discovery 组件不需要与 Unity 服务的任何集成，旨在作为一种完全独立解决方案，用于在局域网上查找要连接的其他游戏。

![图像替代文本](../uploads/Main/NetworkDiscoveryComponent.png)

Inspector 窗口中的 Network Discovery 组件

|**属性**|**功能**|
|:---|:---|
|__Broadcast Port__|用于发送广播和进行监听的网络端口。|
|__Broadcast Key__|要广播的密钥。这应该是唯一值，表示您的发现与其他 Network Discovery 实例的兼容性。唯一广播密钥避免了当不同类型的游戏在同一个局域网上运行时尝试彼此连接。|
|__Broadcast Version__|要包含在广播中的主要版本。将此属性与 Broadcast SubVersion 一起使用可指示版本兼容性。|
|__Broadcast SubVersion__|要包含在广播中的次要版本。将此属性与 Broadcast Version 一起使用可指示版本兼容性。|
|__Broadcast Interval__|指定 Unity 应广播发现信息的频率（以秒为单位）。|
|__Use NetworkManager__|启用此属性可以使用 Network Manager 设置进行广播，然后自动加入找到的游戏。|
|__Broadcast Data__|输入要包含在广播中的自定义数据。如果已启用 Use NetworkManager，则 Network Manager 会重写此属性。|
|__Show GUI__|启用此属性可在播放模式下显示默认广播 GUI。此 GUI 仅用于开发人员测试。|
|__Offset X__|广播 GUI 的 x 轴偏移。仅当启用了 Show GUI 时，此设置才可见。|
|__Offset Y__|广播 GUI 的 y 轴偏移。仅当启用了 Show GUI 时，此设置才可见。|

###在播放模式下运行时

在播放模式下运行时，检视面板中也会显示以下信息：

|**属性** |**功能** |
|:---|:---|
|__hostId__ |用于广播的主机 ID。 |
|__running__ |如果正在广播，则为 true。 |
|__isServer__ |如果作为服务器广播，则为 true。 |
|__isClient__ |如果作为客户端监听广播，则为 true。 |
|__broadcastsReceived__ |收到的广播消息的列表。 |
