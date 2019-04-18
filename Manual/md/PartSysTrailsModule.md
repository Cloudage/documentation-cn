# Trails 模块

使用此模块可将轨迹添加到一部分粒子。此模块与[轨迹渲染器 (Trail Renderer)](class-TrailRenderer.html) 组件共享许多属性，但提供了将轨迹轻松附加到粒子以及从粒子继承各种属性的功能。轨迹可用于各种效果，例如子弹、烟雾和魔法视觉效果。

![处于 __Particles__ 模式的 Trails 模块](../uploads/Main/PartSysTrailsModule.png)

![处于 __Ribbon__ 模式的 Trails 模块](../uploads/Main/PartSysTrailsModuleRibbon.png)

## 属性



|**属性：** |**功能：** |
|:---|:---|
|__Mode__| 选择如何为粒子系统生成轨迹。<br/>- __Particle__ 模式可创建每个粒子在自身路径中留下固定轨迹的效果。<br/>- __Ribbon__ 模式可创建根据存活时间连接每个粒子的轨迹带。 |
|__Ratio__| 一个介于 0 和 1 之间的值，表示已分配轨迹的粒子的比例。Unity 随机分配轨迹，因此该值表示概率。 |
| __Lifetime__ | 轨迹中每个顶点的生命周期，表示为所属粒子的生命周期的乘数。当每个新顶点添加到轨迹时，该顶点将在其存在时间超过其总生命周期后消失。 |
| __Minimum Vertex Distance__| 定义粒子在其轨迹接收新顶点之前必须经过的距离。 |
| __World Space__| 启用此属性后，即便使用 __Local Simulation Space__，轨迹顶点也不会相对于粒子系统的游戏对象移动。相反，轨迹顶点将被置于世界空间中，并忽略粒子系统的任何移动。 |
| __Die With Particles__| 如果选中此框，轨迹会在粒子死亡时立即消失。如果未选中此框，则剩余的轨迹将根据自身的剩余生命周期自然到期。 |
| __Ribbon Count__| 选择要在整个粒子系统中渲染的轨迹带数量。值为 1 将创建连接每个粒子的单个轨迹带。但是，大于 1 的值将创建连接每第 N 个粒子的轨迹带。例如，使用值 2 时，将有一条轨迹带连接粒子 1、3、5，另一条轨迹带连接粒子 2、4、6，以此类推。粒子的排序取决于它们的存活时间。 |
| __Split Sub Emitter Ribbons__| 在用作子发射器的系统上启用此属性时，从同一父系统粒子生成的粒子将共享一个轨迹带。 | 
| __Texture Mode__| 选择应用于轨迹的纹理是沿其整个长度拉伸，还是重复每 N 个距离单位。重复率是基于 __Material__ 中的 __Tiling__ 参数进行控制的。 | 
| __Size affects Width__| 如果启用此属性（选中复选框），则轨迹宽度受粒子大小影响。 |
| __Size affects Lifetime__| 如果启用此属性（选中复选框），则轨迹生命周期受粒子大小影响。|
| __Inherit Particle Color__| 如果启用此属性（选中复选框），则轨迹颜色由粒子颜色调制。 |
| __Color over Lifetime__| 通过一条曲线控制整个轨迹在其附着粒子的整个生命周期内的颜色。 |
| __Width over Trail__| 通过一条曲线控制轨迹沿其长度的宽度。 |
| __Color over Trail__| 通过一条曲线控制轨迹沿其长度的颜色。 |
| __Generate Lighting Data__| 通过启用此属性（选中复选框），可在构建轨迹几何体时包含法线和切线。这样允许它们使用具有场景光照的材质，例如通过标准着色器，或通过使用自定义着色器。 |


## 提示

* 使用 Renderer 模块指定轨迹材质 (Trail Material)。
* Unity 在每个顶点处从颜色渐变 (Color Gradient) 中采样颜色，并在每个顶点之间进行线性的颜色插值。向线渲染器 (Line Renderer) 添加更多顶点可以更接近详细的颜色渐变。

----
*  <span class="page-edit">2017-10-26  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

*  <span class="page-history">在 Unity [2017.1](../Manual/30_search.html?q=newin20171) 中添加了 __Size affects Width__、__Size affects Lifetime__、__Color over Lifetime__、__Width over Trail__、__Color over Trail__ 和 __Generate Lighting Data__ <span class="search-words">NewIn20171</span></span>

*  <span class="page-history">在 Unity [2017.3](../Manual/30_search.html?q=newin20173) 中添加了 __Particle__ 模式 <span class="search-words">NewIn20173</span></span>
