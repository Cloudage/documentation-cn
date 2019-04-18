# Animator 组件 (Animator Component)

Animator 组件用于将动画分配给场景中的游戏对象。Animator 组件需要引用 [Animator Controller](class-AnimatorController.html)，后者定义要使用哪些动画剪辑，并控制何时以及如何在动画剪辑之间进行混合和过渡。

如果游戏对象是具有[Avatar](class-Avatar.html)定义的人形角色，还应在此组件中分配Avatar，如下所示：

![已分配控制器和 Avatar 的 Animator 组件。](../uploads/Main/MecanimAnimatorComponent.png)

下图显示了如何将各种资源（动画剪辑、Animator Controller 和 Avatar）一起汇集在游戏对象的 Animator 组件中：

![此图显示了动画系统的各个部分如何连接在一起](../uploads/Main/MecanimHowItFitsTogether.png)

另请参阅[状态机](AnimationStateMachines.html)、[混合树](class-BlendTree.html)、[Avatar](class-Avatar.html)、[Animator Controller](class-AnimatorController.html)


## 属性

|**_属性：_** ||**_功能：_** |
|:---|:---|:---|
|__Controller__ ||附加到此角色的 Animator Controller。|
|__Avatar__ ||此角色的[Avatar](class-Avatar.html)。（如果 Animator 用于对人形角色进行动画化） |
|__Apply Root Motion__ ||我们应该从动画本身还是从脚本控制角色的位置和旋转。 |
|__Update Mode__||此选项允许您选择 Animator 何时更新以及应使用哪个时间标度。|
||__Normal__| Animator 与 Update 调用同步更新，Animator 的速度与当前时间标度匹配。如果时间标度变慢，动画将通过减速来匹配。|
||__Animate Physics__| Animator 与 FixedUpdate 调用同步更新（即，与物理系统步调一致）。如果要对具有物理交互的对象（例如可四处推动刚体对象的角色）的运动进行动画化，应使用此模式。|
||__Unscaled Time__| Animator 与 Update 调用同步更新，但是 Animator 的速度忽略当前时间标度而不顾一切以 100% 速度进行动画化。此选项可用于以正常速度对 GUI 系统进行动画化，同时将修改的时间标度用于特效或暂停游戏。|
|__Culling Mode__||您可以为动画选择的剔除模式。|
||__Always Animate__ | 始终进行动画化，即使在屏幕外也不要剔除。|
||__Cull Update Transforms__| 未显示渲染器时，禁用变换组件的重定向、IK（反向动力学）和写入。 |
||__Cull Completely__ | 未显示渲染器时，完全禁用动画。 |


## 动画曲线信息

Animator 组件底部的信息框为您提供 Animator Controller 使用的所有剪辑中所用数据的明细。

动画剪辑包含“曲线”形式的数据；曲线表示值如何随时间变化。这些曲线可描述对象的位置或旋转、人形动画系统中肌肉的弯曲或者剪辑内的其他动画值（例如改变的材质颜色）。

下表说明了每个数据项代表的内容：

|**_标签_** |**_描述_** |
|:---|:---|
|__Clip Count__|分配给此 Animator 的 Animator Controller 使用的动画剪辑总数。|
|__Curves (Pos, Rot & Scale)__|用于动画化对象位置、旋转或缩放的曲线总数。这些曲线用于不属于标准人形骨架的动画对象。在对人形 Avatar 进行动画化时，这些曲线会显示额外非肌肉骨骼（例如尾巴、飘逸的布料或悬垂的吊坠）的计数。如果您有人形动画并发现意外的非肌肉动画曲线，表示动画文件中可能有不必要的动画曲线。|
|__Muscles__|此 Animator 用于人形动画的肌肉动画曲线数量。这些是用于对标准人形 Avatar 肌肉进行动画化的曲线。除了 Unity 的标准 Avatar 中所有人形骨骼的标准肌肉运动外，此处还包括用于存储根运动位置和旋转动画的两条“肌肉曲线”。|
|__Generic__|由 Animator 用于动画化其他属性（如材质颜色）的数字（浮点）曲线数量。|
|__PPtr__|精灵动画曲线（由 Unity 的 2D 系统使用）的总数|
|__Curves Count__|动画曲线的合计总数|
|__Constant__|优化为常量（不变）值的动画曲线数量。如果动画文件包含了具有不变值的曲线，Unity 会自动选择此项。|
|__Dense__|使用“密集”数据（通过线性内插的离散值）存储方法进行优化的动画曲线数量。与“流”方法相比，此方法使用的内存少得多。|
|__Stream__|使用“流”数据（这些值具有用于曲线插值的时间和切线数据）存储方法的动画曲线数量。与“密集”方法相比，此数据占用的内存多得多。|

如果导入动画剪辑时在[动画导入引用](class-AnimationClip.html)中将“Anim Compression”设置为“Optimal”，Unity 将使用启发式算法来确定最好使用密集还是流方法来存储每条曲线的数据。

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
