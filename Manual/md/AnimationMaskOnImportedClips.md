# 遮罩

通过遮罩功能可以丢弃剪辑中的一些动画数据，从而让剪辑仅动画化对象或角色的某些部分而不是整体。例如，如果有一个带有投掷动画的角色，现在希望能够将投掷动画与各种其他身体动作（如奔跑、蹲伏和跳跃）结合使用，则可以为投掷动画创建一个遮罩，使其局限于右臂、上半身和头部。然后，动画的这一部分可在基本奔跑或跳跃动画的上一层播放。

遮罩可应用于您的构建，使文件大小和内存更小。此外还可以提高处理速度，因为在运行时混合的动画数据更少。在某些情况下，导入遮罩可能对您不适用。在这种情况下，可使用 Animator Controller 的层设置在运行时应用遮罩。本页面与导入设置中的遮罩有关。

要将遮罩应用于导入的动画剪辑，请展开 Mask 标题以显示 Mask 选项。打开该菜单时，您将看到三个选项：Definition、Humanoid 和 Transform。

![Mask Definition、Humanoid 和 Transform 选项](../uploads/Main/AnimationInspectorMaskOptions.png)

## Definition

在此处可以指定是否要在 Inspector 中专门为此剪辑创建一次性遮罩，或者是否要使用项目中的现有遮罩资源。

如果希望仅为此剪辑创建一次性遮罩，请选择“Create From This Model”。

如果希望多个剪辑使用相同的遮罩，则应选择“Copy From Other Mask”并使用遮罩资源。这样即可对多个剪辑重复使用同一个遮罩定义。

选择 Copy From Other Mask 时，Humanoid 和 Transform 选项不可用，因为这些选项仅与在此剪辑的 Inspector 中创建一次性遮罩有关。

![此处选择了 Copy From Other Mask 选项，并已分配 Mask 资源](../uploads/Main/AnimationInspectorMaskCopyFromOther.png)


## Humanoid

Humanoid 选项可让您选择或取消选择人体图的身体部位，从而快速定义遮罩。如果已将动画标记为人形并具有有效的 Avatar，则可以使用此类选项。

![Humanoid 遮罩选择选项](../uploads/Main/AnimationInspectorMaskHumanoidSelection.png)


## Transform

使用此选项可根据各个骨骼或动画的移动部件来指定遮罩。这样可以更精确地控制确切的遮罩定义，还可以将遮罩应用于非人形动画剪辑。

![Humanoid 遮罩选择选项](../uploads/Main/AnimationInspectorMaskTransformSelection.png)


---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
