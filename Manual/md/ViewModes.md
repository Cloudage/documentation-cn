#Scene 视图控制栏

在 Scene 视图控制栏中可以选择用于查看场景的各种选项，还可以控制是否启用光照和音频。这些控件仅在开发期间影响 Scene 视图，对构建的游戏没有影响。

![](../uploads/Main/SceneViewControlBar.png) 

##绘制模式 (Draw mode) 菜单

第一个下拉菜单选择要用于描绘场景的__绘制模式__。可用选项为：

###Shading mode
* __Shaded__：显示表面时使纹理可见。
* __Wireframe__：使用线框表示形式绘制网格。
* __Shaded Wireframe__：显示网格纹理并叠加线框。


###Miscellaneous
* __Shadow Cascades__：显示方向光[阴影级联](DirLightShadows.html)。
* __Render Paths__：使用颜色代码显示每个对象的[渲染路径](RenderingPaths.html)：蓝色表示[延迟着色](RenderTech-DeferredShading.html)，绿色表示[延迟光照](RenderTech-DeferredLighting.html)，黄色表示[前向渲染](RenderTech-ForwardRendering.html)，红色表示[顶点光照](RenderTech-VertexLit.html)。
* __Alpha Channel__：用 Alpha 渲染颜色。
* __Overdraw__：将对象渲染为透明的“轮廓”。透明的颜色会累积，因此可以轻松找到一个对象绘制在另一个对象上的位置。
* __Mipmaps__：使用颜色代码显示理想的纹理大小：红色表示纹理大于必要值（在当前距离和分辨率下）；蓝色表示纹理可以更大。当然，理想的纹理大小取决于游戏运行的分辨率以及摄像机与特定表面的接近程度。


###Deferred

通过这些模式，可以单独查看 G 缓冲区的每个元素（__Albedo__、__Specular__、__Smoothness__ 和 __Normal__）。请参阅有关[延迟着色](RenderTech-DeferredShading.html)的文档以了解更多信息。

###Global Illumination

可使用以下模式来可视化[全局光照](GlobalIllumination.html)系统的各个方面：__UV Charts__、__Systems__、__Albedo__、__Emissive__、__Irradiance__、__Directionality__、__Baked__、__Clustering__ 和 __Lit Clustering__。请参阅有关 [GI 可视化](GIVis.html)的文档以了解每种模式。

### Material Validator

__材质验证器 (Material Validator)__ 有两种模式：__Albedo__ 和 __Metal Specular__。使用这些模式可以检查基于物理的材质是否使用建议范围内的值。请参阅[基于物理的材质验证器](MaterialValidator.html)以了解更多信息。

##2D、Lighting 和 Audio 开关

在 __Render Mode__ 菜单的右侧有三个按钮，用于打开或关闭 Scene 视图的某些选项：

* __2D__：在场景的 2D 和 3D 视图之间切换。在 2D 模式下，摄像机朝向正 z 方向，x 轴指向右方，y 轴指向上方。
* __Lighting__：打开或关闭 Scene 视图光照（光源、对象着色等）。
* __Audio__：打开或关闭 Scene 视图音频效果。

##Effects 按钮和菜单

该菜单（由 __Audio__ 按钮右侧的小山丘图标激活）具有在 Scene 视图中启用或禁用渲染效果的选项。

* __Skybox__：在场景的背景中渲染的天空盒纹理
* __Fog__：视图随着与摄像机之间的距离变远而逐渐消褪到单调颜色。
* __Flares__：光源上的镜头光晕。
* __Animated Materials__：定义动画化的材质是否显示动画

__Effects__ 按钮本身充当一次性启用或禁用所有效果的开关。

##Gizmos 菜单

Gizmos 菜单包含用于控制对象、图标和辅助图标的显示方式的许多选项。此菜单在 Scene 视图和 [Game 视图](GameView.html)中均可用。请参阅有关 [Gizmos 菜单](GizmosMenu.html)手册页的文档以了解更多信息。

##搜索框

控制栏上最右边的控制项是一个搜索框，可按照名称和/或类型过滤 Scene 视图中的项（可使用搜索框左侧的小菜单对此进行选择）。与搜索过滤条件匹配的项集合也将显示在 Hierarchy 视图中（默认情况下，该视图位于 Scene 视图的左侧）。
