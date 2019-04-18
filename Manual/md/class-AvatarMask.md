# Avatar Mask 窗口

可通过以下两种方法来定义应该对动画的哪些部分进行遮罩：

* 从[人形身体映射图](#Humanoid)进行选择
* 从 [Transform 层级视图](#Transform)中选择要包含或排除的骨骼


<a name="Humanoid"></a> 
## 人形身体选择

如果动画使用人形 Avatar，可以选择是否简化人形身体图的某些部分，从而指示需要遮罩的动画位置：

![使用人形身体定义 Avatar 遮罩 (Avatar Mask)](../uploads/Main/AvatarMaskInspectorHumanoid.png)

身体图将身体部位分为以下部分：

* 头 (Head)
* 左臂 (Left Arm)
* 右臂 (Right Arm)
* 左手 (Left Hand)
* 右手 (Right Hand)
* 左腿 (Left Leg)
* 右腿 (Right Leg)
* 根（Root，由脚下的“阴影”表示）

要包含某个身体部位的动画，请在 Avatar 图中单击该部位，直到该部位显示为绿色。
要排除动画，请单击该身体部位，直到显示为红色。
要包含或排除所有部位，请双击 Avatar 周围的空白区域。
 
此外还可切换手和脚的__反向动力学 (Inverse Kinematics, IK)__，此选项决定了是否在动画混合中包含 IK 曲线。


<a name="Transform"></a> 
## 选择变换组件

另一方面，如果动画不使用人形 Avatar，或者需要更精确地控制对各个骨骼的遮罩，则可以选择或取消选择模型层级视图的相应部分：

1.为需要遮罩变换组件的 Avatar 分配引用。
2.单击 __Import Skeleton__ 按钮。Avatar 的层级视图将显示在检视面板中。
3.可以在层级视图中选中需要遮罩的每个骨骼。

![使用 Transform 方法定义 Avatar 遮罩](../uploads/Main/AvatarMaskInspectorTransform.png)

当指定[动画层](AnimationLayers.html)在运行时应用遮罩，或者在动画文件的导入设置中将遮罩应用于导入动画时，可在 [Animator Controller](AnimatorControllers.html) 中使用遮罩资源。

使用遮罩的好处在于有助于减少内存开销，因为非活动身体部位不需要关联的动画曲线。此外，在播放期间不需要计算未使用的曲线，因此还有助于减少动画的 CPU 开销。

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
