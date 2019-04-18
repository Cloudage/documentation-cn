# 导航网格障碍物 (Nav Mesh Obstacle)

__导航网格障碍物 (Nav Mesh Obstacle)__ 组件允许您描述[导航网格代理](class-NavMeshAgent.html)在世界中导航时应避开的移动障碍物（例如，由物理系统控制的木桶或板条箱）。当障碍物正在移动时，导航网格代理会尽力避开它。当障碍物静止时，它会在导航网格中雕刻一个孔。导航网格代理随后将改变它们的路径以绕过障碍物，或者如果障碍物导致路径被完全阻挡，则寻找其他不同路线。

![](../uploads/Main/NavMeshObstacle.png) 

| **属性**| **功能** |
|:---|:---| 
| __Shape__| 障碍物几何体的形状。选择最适合对象形状的选项。 |
|&nbsp;&nbsp;&nbsp;&nbsp;__Box__|  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Center| 盒体的中心（相对于变换位置）。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Size| 盒体的大小。 |
| &nbsp;&nbsp;&nbsp;&nbsp;__Capsule__|  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Center| 胶囊体的中心（相对于变换位置）。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Radius| 胶囊体的半径。 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Height| 胶囊体的高度。 |
| __Carve__| 勾选 Carve 复选框后，导航网格障碍物会在导航网格中创建一个孔。 |
| &nbsp;&nbsp;&nbsp;&nbsp;Move Threshold| 当导航网格障碍物的移动距离超过 Move Threshold 设置的值时，Unity 会将其视为移动状态。使用此属性可设置该阈值距离来更新移动的雕孔。 |
| &nbsp;&nbsp;&nbsp;&nbsp;Time To Stationary| 将障碍物视为静止状态所需等候的时间（以秒为单位）。 |
| &nbsp;&nbsp;&nbsp;&nbsp;Carve Only Stationary| 启用此属性后，只有在静止状态时才会雕刻障碍物。请参阅下面的[移动的导航网格障碍物的逻辑](#LogicMovingObstacles)以了解更多信息。 |

##详细信息

![](../uploads/Main/NavMeshObstacleCarving.svg) 

导航网格障碍物可通过两种方式影响导航网格代理在游戏中的导航：

### 障碍

未启用 __Carve__ 时，导航网格障碍物的默认行为类似于碰撞体的行为。导航网格代理会尝试避免与导航网格障碍物的碰撞，当靠近时，它们会与导航网格障碍物碰撞。障碍躲避行为是非常基本的，具有一条短半径。因此，导航网格代理可能无法在导航网格障碍物很混乱的环境中找到方向。此模式最适合用于障碍物不断移动的情况（例如，车辆或玩家角色）。

### 雕刻

启用 __Carve__ 时，障碍物处于静止状态时将在导航网格中雕刻一个孔。移动时，障碍物即为障碍物。在导航网格中雕刻一个孔后，寻路器 (pathfinder) 能够让导航网格代理绕过雕有障碍物的位置周围，或者如果当前路径被障碍物阻挡，则寻找另一条路线。对于通常会阻碍导航但可被玩家或其他游戏事件（如爆炸）移动的导航网格障碍物（例如板条箱或木桶），最好为其开启雕刻功能。

![](../uploads/Main/NavMeshObstacleTrap.svg) 

<a name="LogicMovingObstacles"> </a> 

## 移动的导航网格障碍物的逻辑

当导航网格障碍物的移动距离超过 __Carve__ > __Move Threshold__ 设置的值时，Unity 会将其视为移动状态。当导航网格障碍物移动时，雕刻的孔也会移动。但是，为了减少 CPU 开销，只在必要时才重新计算该孔。此计算的结果将在下一帧更新中可用。重新计算逻辑有两个选项：

* 仅当导航网格障碍物静止时才进行雕刻

* 导航网格障碍物移动后进行雕刻

###仅当导航网格障碍物静止时才进行雕刻

这是默认行为。要启用此选项，请勾选导航网格障碍物组件的 __Carve Only Stationary__ 复选框。在此模式下，当导航网格障碍物移动时，雕刻的孔将被移除。当导航网格障碍物已停止移动并且静止时间超过 __Carving Time To Stationary__ 设置的值时，障碍物将被视为静止状态，并再次更新雕刻的孔。当导航网格障碍物为移动状态时，导航网格代理会使用碰撞躲避功能避开它，但不会在它周围规划路径。

__Carve Only Stationary__ 通常是性能方面的最佳选择，当与导航网格障碍物相关联的游戏对象由物理系统控制时，是很适合的选项。

###导航网格障碍物移动后进行雕刻

要启用此模式，请取消勾选导航网格障碍物组件的 __Carve Only Stationary__ 复选框。取消勾选此复选框的情况下，当障碍物移动的距离超过 __Carving Move Threshold__ 设置的值时，雕刻的孔会更新。此模式适用于大型缓慢移动的障碍物（例如，步兵避开的坦克）。

**注意**：使用导航网格查询方法时，应考虑到更改导航网格障碍物与该更改对导航网格生效之间存在一帧延迟。

## 另请参阅

1.[创建导航网格障碍物](nav-CreateNavMeshObstacle.html) - 介绍如何创建导航网格障碍物。

2.[导航系统的内部工作原理](nav-InnerWorkings.html) - 详细了解如何将导航网格障碍物用作导航的一部分。

3.[导航网格障碍物脚本参考](../ScriptReference/AI.NavMeshObstacle.html) - 导航网格障碍物脚本 API 的完整描述。

