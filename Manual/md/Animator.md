Animator Controller资源
================================

当您准备好使用动画剪辑时，需要使用__Animator Controller__使这些剪辑结合在一起。Animator Controller资源是在 Unity 内创建的，允许您为角色或对象维护一组动画。

![项目文件夹中的Animator Controller资源](../uploads/Main/AnimatorAssetIcon.png)

从 Assets 菜单或从 Project 窗口中的 Create 菜单创建Animator Controller资源。

在大多数情况下，拥有多个动画并在满足某些游戏条件时在这些动画之间切换是很常见的。例如，只要按下空格键，就可以从行走动画切换到跳跃动画。但是，即使您只有一个动画剪辑，仍需要将其放入 Animator Controller 以便将其用于游戏对象。

控制器使用所谓的__状态机__来管理各种动画状态和它们之间的过渡；状态机可视为一种流程图，或者是在 Unity 中使用可视化编程语言编写的简单程序。可在[此处](AnimationStateMachines.html)找到有关状态机的更多信息。可在 [Animator 窗口](AnimatorWindow.html)中创建、查看和修改Animator Controller的结构。

![简单的Animator Controller](../uploads/Main/MecanimAnimatorControllerWindow.png)

最终会通过连接__Animator__组件（其中引用了Animator Controller）将Animator Controller应用于对象。请参阅关于 [Animator](class-Animator.html) 组件和 [Animator Controller](class-AnimatorController.html) 的参考手册页面来了解有关其用法的更多详细信息。


