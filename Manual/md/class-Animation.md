# 动画

这是**旧版**动画 (Animation) 组件，在引入 Unity 的当前动画系统之前，此组件在游戏对象上用于动画目的。

此组件保留在 Unity 中仅为了确保向后兼容。对于新项目，请使用[Animator组件](class-Animator.html)。

![动画检视面板 (Animation Inspector)](../uploads/Main/AnimationInspector35.png)


## 属性

|**_属性：_** ||**_功能：_** |
|:---|:---|:---|
|__Animation__ ||启用 __Play Automatically__ 的情况下播放的默认动画。 |
|__Animations__ ||可从脚本访问的动画列表。 |
|__Play Automatically__ ||启用此选项可在游戏开始时自动播放动画。 |
|__Animate Physics__ ||启用此选项可使动画与物理系统交互。 |
|__Culling Type__||确定何时不播放动画。|
||__Always Animate__|始终进行动画化。|
||__Based on Renderers__|基于默认动画姿势进行剔除。|
||__Based on Clip Bounds__|基于剪辑边界（在导入期间计算）进行剔除。当剪辑边界不在视野范围内时，不播放动画。|
||__Based on User Bounds__|基于用户定义的边界进行剔除。当用户定义的边界不在视野范围内时，不播放动画。|

请参阅 [Animation 窗口指南](AnimationEditorGuide.html)了解有关如何在 Unity 中创建动画的更多信息。
请参阅[模型导入工作流程](ImportingModelFiles.html)页面了解如何导入带动画的角色。

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
