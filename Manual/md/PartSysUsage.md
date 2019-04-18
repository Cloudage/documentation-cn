#在 Unity 中使用粒子系统

Unity 使用组件实现粒子系统，因此将粒子系统放置在场景中涉及到添加预制的游戏对象（菜单：__GameObject__ &gt; __Effects__ &gt; __Particle System__）或将组件添加到现有游戏对象（菜单：__Component__ &gt; __Effects__ &gt; __Particle System__）。由于组件非常复杂，因此 Inspector 分为多个可折叠子部分或__模块__，每个子部分或模块都包含一组相关属性。此外，可使用单独的 Editor 窗口（通过 Inspector 中的 __Open Window__ 按钮访问）同时编辑一个或多个系统。请参阅有关[粒子系统组件](class-ParticleSystem.html)和各个[粒子系统模块](ParticleSystemModules.html)的文档，了解更多信息。

选择带有粒子系统的游戏对象时，Scene 视图包含一个小的 __Particle Effect__ 面板，其中有一些简单控件，用于显示对系统设置的更改。

![](../uploads/Main/PartSysEffectPanel.png) 

__Playback Speed__ 用于加快或减慢粒子模拟速度，可以直观查看在高级状态下的效果。__Playback Time__ 表示自系统启动以来经过的时间；这可能比实时更快或更慢，具体取决于播放速度。__Particle Count__ 表示系统中当前有多少粒子。通过单击 __Playback Time__ 标签并向左和向右拖动鼠标，即可前后移动播放时间。面板顶部的按钮可用于暂停和恢复模拟，或停止模拟并重置为初始状态。

##随时间推移而变化的属性

粒子甚至整个粒子系统的许多数字属性都可能随时间而变化。Unity 提供了几种不同的方法来指定这种变化的发生方式：

* __Constant：__属性的值在其整个生命周期内是固定的。
* __Curve：__该值由曲线/图形指定。
* __Random Between Two Constants：__两个常量值定义了值的上限和下限；实际值随着时间的推移在这些边界之间随机变化。
* __Random Between Two Curves：__两条曲线定义了值在生命周期内给定点的上限和下限；当前值在这些边界之间随机变化。

同样，主模块中的 __Start Color__ 属性具有以下选项：

* __Color：__粒子初始颜色在整个系统的生命周期内是固定的。
* __Gradient：__使用渐变指定的初始颜色发射粒子，渐变表示粒子系统的生命周期。
* __Random Between Two Colors：__选择两种给定颜色之间的随机线性插值作为初始粒子颜色。
* __Random Between Two Gradients：__在对应于系统当前时期的点处从给定渐变中挑选两种颜色；选择这两种颜色之间的随机线性插值作为初始粒子颜色。

对于其他颜色属性，例如 __Color over Lifetime__，有两个单独的选项：

* __Gradient：__颜色值取自表示粒子系统生命周期的渐变。
* __Random Between Two Gradients：__在对应于粒子系统当前时期的点处从给定渐变中挑选两种颜色；选择这两种颜色之间的随机线性插值作为颜色值。

各种模块中的颜色属性按照每个通道相乘，从而计算出最终的粒子颜色结果。

##动画绑定

动画系统可以访问所有粒子属性，这意味着可以将它们设置到关键帧中并从动画中控制它们。

要访问粒子系统的属性，必须有一个 Animator 组件连接到粒子系统的游戏对象。此外还需要动画控制器 (Animation Controller) 和动画。


![要动画化粒子系统，请添加 Animator 组件，并为动画控制器 (Animator Controller) 分配动画。](../uploads/Main/ParticleSystemAnimatorComponent.png)


要动画化粒子系统属性，请打开 __Animation 窗口__，并选择包含 Animator 和粒子系统的游戏对象。单击 __Add Property__ 以添加属性。


![在 Animation 窗口中为动画添加属性。](../uploads/Main/ParticleSystemAnimationWindow.png)


向右滚动以显示__添加控件__。


![](../uploads/Main/ParticleSystemAnimationScrollRight.png) 


请注意，对于曲线，只能对整体__曲线乘数__进行关键帧设置（可在 __Inspector__ 中的曲线编辑器旁边找到该曲线乘数）。

---

* <span class="page-edit">2017-09-19  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 Unity 4.6 中更改了 GameObject 菜单</span>
