#弹簧关节

__弹簧关节 (Spring Joint)__ 将两个[刚体](class-Rigidbody.html)连接在一起，但允许两者之间的距离改变，就好像它们通过弹簧连接一样。

![](../uploads/Main/Inspector-SpringJoint.png) 


##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Connected Body__ |包含弹簧关节的对象连接到的刚体对象。如果未指定对象，则弹簧将连接到空间中的固定点。|
|__Anchor__ |关节在对象的局部空间中所附加到的点。|
|__Auto Configure Connected Anchor__ |Unity 是否应该自动计算连接锚点的位置？  |
|__Connected Anchor__ |关节在连接对象的局部空间中所附加到的点。 |
|__Spring__ |弹簧的强度。|
|__Damper__ |弹簧为活性状态时的压缩程度。 |
|__Min Distance__ |弹簧不施加任何力的距离范围的下限。 |
|__Max Distance__ |弹簧不施加任何力的距离范围的上限。 |
|__Tolerance__ |更改容错。允许弹簧具有不同的静止长度。 |
|__Break Force__ |为破坏此关节而需要施加的力。 |
|__Break Torque__ |为破坏此关节而需要施加的扭矩。 |
|__Enable Collision__ |是否应启用两个连接对象之间的相互碰撞？ |
|__Enable Preprocessing__ | 禁用预处理有助于稳定无法满足的配置。 |

##详细信息

弹簧就像一块弹性物，试图将两个锚点一起拉到完全相同的位置。拉力的强度与两个点之间的当前距离成比例，其中每单位距离的力由 _Spring_ 属性设定。为了防止弹簧无休止振荡，可以设置 _Damper_ 值，从而根据与两个对象之间的相对速度按比例减小弹簧力。值越高，振荡消失的速度越快。

可以手动设置锚点，但如果启用 _Auto Configure Connected Anchor_，Unity 将自动设置连接锚点，以保持它们之间的初始距离（即，在定位对象时在 Scene 视图中设置的距离）。

_Min Distance_ 和 _Max Distance_ 值用于设置弹簧不施加任何力的距离范围。例如，可以使用该距离范围允许对象进行少量的独立移动，但当对象之间的距离太大时将它们拉到一起。

