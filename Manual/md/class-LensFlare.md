镜头光晕 (Lens Flare)
==========


__镜头光晕__模拟在摄像机镜头内折射光线的效果。它们用于表示非常明亮的光源，或者更巧妙地用于为场景添加更多气氛。


![](../uploads/Main/Inspector-LensFlare.png) 

设置镜头光晕的最简单方法就是分配[光源 (Light)](class-Light.html) 的 Flare 属性。Unity 在[标准资源包](AssetTypes.html#Standard)中提供了几个预先配置的光晕。

除此以外，可从菜单栏中通过 __GameObject &gt; Create Empty__ 创建空__游戏对象__，并通过 __Component &gt; Effects &gt; Lens Flare__ 将镜头光晕 (Lens Flare) __组件__添加到该游戏对象。然后，在 Inspector 中选择 __Flare__。

要在 __Scene 视图__中查看镜头光晕的效果，请在 Scene 视图工具栏中选中 __Effect__ 下拉选单，并选择 Flares 选项。


![启用 Fx 按钮以便在 Scene 视图中查看镜头光晕](../uploads/Main/LensFlare-FXButton.png)


属性
----------



|**_属性：_** |**_功能：_** |
|:---|:---|
|__Flare__ |要渲染的[光晕](class-Flare.html)。光晕定义了镜头光晕外观的所有方面。 |
|__Color__ |一些光晕可通过着色更好地适应场景的氛围。 |
|__Brightness__ |镜头光晕的大小和亮度。 |
|__Fade Speed__ |光晕淡化的速度快慢。 |
|__Ignore Layers__ |为不应隐藏光晕的层选择遮罩。 |
|__Directional__ |如果设置此属性，则光晕将沿着游戏对象的正 Z 轴定向。光晕看起来好像是无限远，不会跟踪对象的位置，只跟踪 Z 轴的方向。 |


详细信息
-------


可直接将光晕设置为[光源 (Light)](class-Light.html) 组件的属性，或者将它们单独设置为镜头光晕 (Lens Flare) 组件。如果将它们连接到光源，它们将自动跟踪光源的位置和方向。要获得更精确的控制，请使用此组件。

[摄像机](class-Camera.html)必须连接一个[光晕层 (Flare Layer)](class-FlareLayer.html) 组件才能使光晕可见（默认情况下就是这样的，因此无需进行任何设置）。


提示
-----



* 谨慎使用镜头光晕。
* 如果使用非常明亮的镜头光晕，请确保其方向与场景的主光源相适应。
* 要设计自己的光晕，您需要创建一些光晕资源。为此，应首先复制我们在标准资源的 __Lens Flares__ 文件夹中提供的一些光晕资源，然后在此基础上进行修改。
* 镜头光晕会被__碰撞体__阻挡。光晕游戏对象和摄像机之间的碰撞体即使没有__网格渲染器__，也会隐藏光晕。如果中间碰撞体被标记为触发器 (Trigger)，当且仅当 __Physics.queriesHitTriggers__ 为 true 时，该碰撞体才会阻挡光晕。
