#Physics 2D Settings


__Physics 2D Settings__ 可用于提供 2D 物理的全局设置（菜单：__Edit__ > __Project Settings__ > __Physics 2D__）。

（还有一个适用于 3D 项目的对应 [Physics Manager](class-PhysicsManager.html)。）

![](../uploads/Main/Physics2DManager.png) 

##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Gravity__ |应用于所有 [2D 刚体](class-Rigidbody2D.html)游戏对象的重力大小。通常，仅为 y 轴的负方向设置重力。 |
|__Default Material__ |在没有为单独 2D 碰撞体分配材质的情况下，使用的默认 [2D 物理材质](class-PhysicsMaterial2D.html)。 |
|__Velocity Iterations__ |物理引擎为处理速度影响而进行的迭代次数。数字越大，物理属性越准确，但代价是 CPU 时间增加。 |
|__Position Iterations__ |物理引擎为处理位置变化而进行的迭代次数。数字越大，物理属性越准确，但代价是 CPU 时间增加。 |
|__Velocity Threshold__ |相对速度低于此值的碰撞被视为非弹性碰撞（即，碰撞的游戏对象不会相互反弹）。 |
|__Max Linear Correction__ |解算约束时使用的最大线性位置校正（范围从 0.0001 到 1000000）。这有助于防止过冲。 |
|__Max Angular Correction__ |解算约束时使用的最大角度校正（范围从 0.0001 到 1000000）。这有助于防止过冲。 |
|__Max Translation Speed__ |任何物理更新期间 2D 刚体游戏对象的最大线性速度。 |
|__Max Rotation Speed__ |The maximum rotation speed of a Rigidbody 2D GameObject during any physics update. |
|__Min Penetration For Penalty__ |在施加任何分离冲击力之前允许的最小接触穿透半径。 |
|__Baumgarte Scale__ |用于确定解算碰撞重叠速度的缩放因子。 |
|__Baumgarte Time of Impact Scale__ |用于确定解算撞击时间重叠速度的缩放因子。  |
|__Time to Sleep__ |在 2D 刚体停止移动之后而进入睡眠状态之前必须经过的时间（以秒为单位）。 |
|__Linear Sleep Tolerance__ |如果低于该线性速度，2D 刚体在经过 __Time to Sleep__ 过后进入睡眠状态。 |
|__Angular Sleep Tolerance__ |如果低于该旋转速度，2D 刚体在经过 __Time to Sleep__ 过后进入睡眠状态。 |
|__Queries Hit Triggers__ |选中此框可让标记为__触发器__的 2D 碰撞体在任何物理查询（如线性投射或射线投射）与它们相交时返回命中。取消选中此框会使这些查询不返回命中。 |
|__Queries Start In Colliders__ | 选中此框可使 2D 碰撞体中启动的物理查询检测是在哪个碰撞体中开始的。 |
|__Change Stops Callbacks__ |选中此框时，如果删除或移动了碰撞中涉及的任何游戏对象，可立即停止报告碰撞回调。 |
|__Layer Collision Matrix__ |定义[基于层的碰撞](LayerBasedCollision.html)检测系统的行为方式。|

##注意

Physics 2D Settings 定义了物理模拟精度的限制。一般来说，更精确的模拟需要更多的处理开销，因此这些设置提供了一种精度与性能的折衷方法。有关更多信息，请参阅本手册的[物理](PhysicsSection.html)部分。
