# Network Proximity Checker

Network Proximity Checker 组件根据与玩家的接近程度来控制游戏对象对于网络客户端的可见性。

![Network Proximity Checker 组件](../uploads/Main/NetworkProximityCheck.png)

|**属性**|**功能**|
|:---|:---|
|**Vis Range**|定义游戏对象的可见范围。|
|**Vis Update Interval**|定义游戏对象应检查玩家是否进入其可见范围的频率（以秒为单位）。|
|**Check Method**|定义要用于接近度检查的物理类型（2D 还是 3D）。|
|**Force Hidden**|勾选此复选框可对所有玩家隐藏此对象。|

使用 Network Proximity Checker 时，客户端上运行的游戏不具有关于不可见游戏对象的信息。因此具有两个主要优势：减少通过网络发送的数据量；让游戏更难以破解。

此组件依赖于物理系统来计算可见性，因此游戏对象上还必须有碰撞体组件。

具有 Network Proximity Checker 组件的游戏对象还必须具有 [Network Identity](class-NetworkIdentity.html) 组件。在某个游戏对象上创建 Network Proximity Checker 组件时，Unity 还将在该游戏对象上创建 Network Identity 组件（如果还没有此组件）。
