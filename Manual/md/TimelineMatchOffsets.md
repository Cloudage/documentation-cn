# 匹配剪辑偏移

每个动画剪辑包含关键动画或运动（用于动画化游戏对象）或人形角色（绑定到动画轨道）。将动画剪辑添加到动画轨道时，动画剪辑的关键动画或运动不会自动匹配动画轨道上的上一个剪辑或下一个剪辑。默认情况下，每个动画剪辑在游戏对象的位置和旋转处开始，对于人形角色，则在时间轴实例的开头处开始。

![例如，三个动画剪辑创建一个动画序列，该序列以一个先站立不动再奔跑的人形动画剪辑开头，然后是一个奔跑并左转的人形动画剪辑，最后是一个奔跑后站立不动的人形动画剪辑](../uploads/Main/timeline_match_prematch_clips.png)

![每个动画剪辑在游戏对象的位置和旋转处开始，对于人形角色，则在时间轴实例的开头处（红色箭头）开始。三个动画剪辑：Stand2Run、RunLeft 和 Run2Stand 都从红色箭头处开始，但分别在绿色、蓝色和黄色箭头处结束。](../uploads/Main/timeline_match_prematch_scene.png)

要让动画序列在相邻的动画剪辑之间无缝流动，必须使每个动画剪辑与上一个或下一个剪辑匹配。匹配剪辑将为每个动画剪辑添加位置和旋转偏移（称为剪辑根运动偏移）。以下部分将介绍如何匹配两个动画剪辑或更多动画剪辑。

## 匹配两个剪辑

要匹配两个剪辑之间的根运动，右键单击要匹配的动画剪辑。从上下文菜单中，选择 Match Offsets to Previous Clip 或 Match Offsets to Next Clip。

![例如，右键单击名为“RunLeft”的中间动画剪辑以将该剪辑的偏移与上一个剪辑或下一个剪辑匹配](../uploads/Main/timeline_match_clip_two.png)

上下文菜单仅显示可用于所点击的动画剪辑的 Match Offset 选项。例如，如果点击的动画剪辑**之前**有空白，则只有 Match Offsets to Next Clip 菜单项可用。

为单个动画剪辑匹配偏移时，不需要首先选择动画剪辑，但必须右键单击要匹配的动画剪辑。例如，如果右键单击未选定的动画剪辑，则会匹配单击的剪辑，并忽略所有选定的动画剪辑。

## 匹配多个剪辑

要匹配多个剪辑的根运动，请选择要匹配的动画剪辑，然后右键单击一个选定剪辑。从上下文菜单中，选择 Match Offsets to Previous Clip 或 Match Offsets to Next Clip。

![例如，选择“RunLeft”和“Run2Stand”剪辑。右键单击其中一个选定的剪辑，然后选择 Match Offsets to Previous Clips，从而将 RunLeft 剪辑与上一个剪辑 Stand2Run 进行匹配，并将 Run2Stand 与上一个剪辑 RunLeft 进行匹配。](../uploads/Main/timeline_match_clip_many.png)

---
* <span class="page-edit">2017-12-07  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

