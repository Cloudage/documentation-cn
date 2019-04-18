2D 弹簧关节 (Spring Joint 2D)
===============


__2D 弹簧关节__组件允许由刚体物理组件控制的两个游戏对象就像通过弹簧连接在一起一样。弹簧将在两个对象之间沿轴线施力，试图使这两个对象保持一定距离。


![](../uploads/Main/SpringJoint2DInspector.png) 


|**_属性：_** |**_功能：_** |
|:---|:---|
|__Enable Collision__ |连接的两个对象能否相互碰撞？选中此复选框表示“能”。|
|__Connected Rigid Body__ |在此处指定该关节连接到的另一个对象。如果将此属性保留为 __None__，此关节的另一端将固定到空间中由 __Connected Anchor__ 设置所定义的点。选择字段右侧的圆圈可查看要连接到的对象的列表。|
|__Auto Configure Connected Anchor__ | 选中此框可为该关节连接到的另一个对象自动设置锚点位置。（选中此框将无需填写 __Connected Anchor__ 字段。） |    

|__Anchor__ |The place (in terms of X, Y co-ordinates on the __RigidBody__) where the end point of the joint connects to *this* object. |
|__Connected Anchor__ |The place (in terms of X, Y co-ordinates on the __RigidBody__) where the end point of the joint connects to *the other* object. |
|__Auto Configure Distance__ | Check this box to automtically detect the distance between the two objects and set it as the distance that the joint keeps between the two objects. |
|__Distance__ |The distance that the spring should attempt to maintain between the two objects. (Can be set manually.) |
|__Damping Ratio__ | The degree to which you want to suppress spring oscillation: In the range 0 to 1, the higher the value, the less movement. |
|__Frequency__ |The frequency at which the spring oscillates while the objects are approaching the separation distance you want (measured in cycles per second): In the range 0 to 1,000,000 - the higher the value, the stiffer the spring. |
|__Break Force__ |Specify the force level needed to break and so delete the joint. __Infinity__ means it is unbreakable. |


详细信息
-------
（另请参阅 [2D 关节](Joints2D.html)中的*详情和提示*以了解所有 2D 关节的有用背景信息。）


此关节的行为像弹簧。目的是保持两点之间的线性距离。通过 __Distance__ 设置可设定此距离。这两个点可以是两个 __2D 刚体__组件，或一个 __2D 刚体__组件和世界中的一个固定位置。（将 __Connected Rigidbody__ 设置为 None，即可连接到世界中的固定位置）。此关节对两个刚体施加线性力。不施加扭矩（角力）。

此关节使用模拟弹簧。可以设置弹簧的刚度和运动：

一个几乎不动的僵硬弹簧…

* 高（1,000,000 为最高）__Frequency__ == 僵硬弹簧。

* 高（1 为最高）__Damping Ratio__ == 几乎不动的弹簧。

一个移动的松弛弹簧...

* 低 __Frequency__ == 松弛弹簧。

* 低 __Damping Ratio__ == 移动的弹簧。

弹簧在对象之间施力时，往往会超过对象之间设定的距离，然后反复反弹，产生连续振荡。__Damping Ratio__ 用于设置对象在多长时间内停止移动。__Frequency__ 用于设置对象在目标距离的任何一侧振荡的速度。

此关节有一个约束：

* 将两个刚体对象上的两个锚点之间的线性距离保持为零。

**例如：**

使用此关节构建的物理对象就好像是使用弹簧或允许旋转的连接方式连接在一起一样。例如：

* 角色的身体由多个似乎半刚性的对象组成。使用弹簧关节可将角色的身体部位连接在一起，允许这些部位相互弯曲。还可以指定身体部位松散还是紧密结合在一起。


**提示：**

* __Frequency__ 为零是特殊情况：这种情况下会产生最大刚度的弹簧。
* 2D 弹簧关节使用 2D 盒体弹簧关节。
2D 距离关节也使用相同的 2D 盒体弹簧关节，但会将频率设置为零！在技术上，频率为零且阻尼为 1 的 2D 弹簧关节与 2D 距离关节相同！





