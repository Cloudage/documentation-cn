创建AnimatorController
==============================

###Animator Controller

可从__Animator Controller__视图（菜单：__Window &gt; Animator Controller__）中查看并设置角色行为。

可通过多种方式创建Animator Controller：

* 从 __Project 视图__中，选择“__Create &gt; Animator Controller__”。

* 在 Project 视图中右键单击并选择“__Create &gt; Animator Controller__”。

* 从 Assets 菜单中，选择“__Assets &gt; Create &gt; Animator Controller__”。



随后将在磁盘上创建 `.controller` 资源。在 __Project Browser__ 窗口中，该图标如下所示：

![磁盘上的Animator Controller资源](../uploads/Main/MecanimAnimatorControllerIcon.png)

###Animator 窗口
完成状态机设置后，您可以将控制器放在 __Hierarchy 视图__中具有 Avatar 的任何角色的 Animator 组件上。

Animator Controller 窗口包含：

* __动画层小部件__（左上角，请参阅[动画层](AnimationLayers.html)）
* __事件参数小部件__（左上方，请参阅[动画参数](AnimationParameters.html)）
* [状态机](AnimationStateMachines.html)可视化。

![Animator Controller 窗口](../uploads/Main/MecanimAnimatorControllerWindow.png)

请注意，Animator Controller 窗口将始终显示最近选择的 `.controller` 资源的状态机（无论当前加载了什么场景）。




