#网格外链接 (Off-Mesh Link)

OffMeshLink 组件允许您合并无法使用可行走表面来表示的导航捷径。例如，跳过沟渠或围栏，或在通过门之前打开门，全都可以描述为网格外链接。

##属性

![](../uploads/Main/OffMeshLink.png) 

|属性 |功能 |
|:---|:---|
|**Start** |描述网格外链接起始位置的对象。 |
|**End** |描述网格外链接起始位置的对象。|
|**Cost Override**|如果值为正，则在计算处理路径请求的路径成本时使用该值。否则，使用默认成本（此游戏对象所属区域的成本）。如果 Cost Override 设置为值 3.0，则在网格外链接上移动的成本将是在默认导航网格区域上移动相同距离的成本的三倍。如果希望让代理通常优先选择步行，但当步行距离明显更长时使用网格外链接，则 Cost Override 设置将变得有用。 |
|**Bi-Directional**|如果启用此属性，则可以在任一方向上遍历链接。否则，只能按照从 _Start_ 到 _End_ 的方向遍历链接。|
|**Activated** |指定寻路器 (pathfinder) 是否将使用此链接（如果将此属性设置为 false，则将忽略它）。 |
|**Auto Update Positions** |如果启用此属性，当端点移动时，网格外链接将重新连接到导航网格。如果禁用，即使移动了端点，链接也将保持在其起始位置。 |
|**Navigation Area** |描述链接的[导航区域类型](nav-AreasAndCosts.html)。该区域类型允许您对相似区域类型应用常见的遍历成本，并防止某些角色根据代理的区域遮罩 (Area Mask) 访问网格外链接。 |

##详细信息

![](../uploads/Main/OffMeshLinkDebug.svg) 

如果代理未遍历网格外链接，请确保两个端点都已正确连接。正确连接的端点应在接入点周围显示一个圆圈。

另一个常见原因是导航网格代理 (Navmesh Agent) 的 _Area Mask_ 没有包含网格外链接的区域。

###阅读更多信息
- [创建网格外链接](nav-CreateOffMeshLink.html) – 关于设置网格外链接的工作流程。
- [自动构建网格外链接](nav-BuildingOffMeshLinksAutomatically.html) - 如何自动创建。
- [网格外链接脚本参考](../ScriptReference/AI.OffMeshLink.html) - 网格外链接脚本 API 的完整描述。
