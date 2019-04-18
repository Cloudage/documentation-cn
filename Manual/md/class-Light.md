# Light Inspector

光源决定对象的着色及其投射的阴影。因此，它们是图形渲染的基本部分。请参阅关于[光照](LightingOverview.html)和[全局光照](GlobalIllumination.html)的文档以了解关于 Unity 中的光照概念的更多详细信息。

![](../uploads/Main/class-Light-0.png) 

## 属性

| 属性：| 功能： |
|:---|:---| 
| __Type__| 当前的光源类型。可能的值为 __Directional__、__Point__、__Spot__ 和 __Area__（请参阅[光照概述](Lighting.html)以了解这些类型的详细信息）。 |
| __Range__| 定义从对象中心发出的光线的行进距离（仅限__点光源__和__聚光灯__）。 |
| __Spot Angle__| 定义聚光灯锥形底部的角度（以度为单位）（仅限__聚光灯__）。 |
| __Color__| 使用拾色器来设置发光的颜色。 |
| __Mode__| 指定[光照模式](LightModes.html)，此模式用于确定是否以及如何“烘焙”光源。模式可能为 __Realtime__、__Mixed__ 和 __Baked__。请参阅关于[实时光照](LightMode-Realtime.html)、[混合光照](LightMode-Mixed.html)和[烘焙光照](LightMode-Baked.html)的文档以了解更多详细信息。 |
| __Intensity__| 设置光源的亮度。__方向光__的默认值为 0.5。__点光源__、__聚光灯__或__面光源__的默认值为 1。  |
| __Indirect Multiplier__| 使用此值可改变间接光的强度。间接光是从一个对象弹射到另一个对象的光。__Indirect Multiplier__ 定义由全局光照 (GI) 系统计算的散射光的亮度。如果将 __Indirect Multiplier__ 设置为低于 __1__ 的值，每次反弹都会使散射光变得更暗。大于 __1__ 的值使光线在每次弹射之后更明亮。例如，将阴暗处的阴暗面（例如洞穴内部）变亮到能够清晰可见，这个非常有用。或者，如果要使用[实时全局光照](GlobalIllumination.html)，但是希望限制单一实时光源以便它只发出直射光，请将其 __Indirect Multiplier__ 设置为 __0__。 |
| __Shadow Type__| 决定此光源投射生硬阴影、柔和阴影还是根本不投射阴影。请参阅有关[阴影](ShadowOverview.html)的文档以了解关于硬阴影以及软阴影的信息。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Baked Shadow Angle| 如果 __Type__ 设置为 __Directional__ 且 __Shadow Type__ 设置为 __Soft Shadows__，此属性将为阴影边缘添加一些人工柔化，使其看起来更自然。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Baked Shadow Radius| 如果 __Type__ 设置为 __Point__ 或 __Spot__ 且 __Shadow Type__ 设置为 __Soft Shadows__，此属性将为阴影边缘添加一些人工柔化，使其看起来更自然。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Realtime Shadows| 这些属性在 __Shadow Type__ 设置为 __Hard Shadows__ 或 __Soft Shadows__ 时可用。使用这些属性可控制实时阴影渲染设置。 |
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Strength| 使用滑动条来控制此光源所投射阴影的暗度（以 0 和 1 之间的值表示）。默认情况下，此值设置为 1。 |
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Resolution| 控制阴影贴图的渲染分辨率。较高的分辨率会增加阴影的保真度，但需要更多的 GPU 时间和内存使用量。 |
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bias| 使用滑动条来控制阴影被推离光源的距离（定义为 0 到 2 之间的值）。这可用于避免错误的自阴影瑕疵。请参阅阴影贴图和 Bias 属性以了解更多信息。默认情况下，该值设置为 0.05。 |
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Normal Bias| 使用滑动条来控制阴影投射面沿着表面法线收缩的距离（定义为 0 到 3 之间的值）。这可用于避免错误的自阴影瑕疵。请参阅关于阴影贴图和 Bias 属性的文档以了解更多信息。默认情况下，该值设置为 0.4。<br/> |
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Near Plane| 使用滑动条来控制渲染阴影时近裁剪面的值，定义为介于 0.1 和 10 之间的值。此值被限制为光源的 __Range__ 属性的 0.1 个单位或 1％（以较低者为准）。默认情况下，该值设置为 0.2。 |
| __Cookie__| 指定用于投射阴影的纹理遮罩（例如，为光源创建轮廓或图案光照）。 |
| __Draw Halo__| 勾选此框可绘制直径等于 __Range__ 值的光源的球形[光环 (Halo)](class-Halo.html)。您还可以使用 Halo 组件来实现此效果。请注意，除了光源 (Light) 组件中的光环外，还绘制 Halo 组件，并且 Halo 组件的 __Size__ 参数将确定其半径，而不是直径。 |
| __Flare__| 如果要设置[光晕](class-Flare.html)在光源位置渲染，请将资源置于此字段中以用作其源。 |
| __Render Mode__| 使用此下拉选单来设置所选光源的渲染优先级。这会影响光照保真度和性能（请参阅下文的*性能注意事项*）。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Auto| 在运行时确定渲染方法，具体取决于附近光源的亮度和当前的[质量设置 (Quality Settings)](class-QualitySettings.html)。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Important| 光源始终以像素质量为单位进行渲染。__Important__ 模式仅用于最显著的视觉效果（例如，玩家汽车的前照灯）。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Not Important| 光源以快速、顶点/对象模式进行渲染。  |
| __Culling Mask__| 使用此属性可选择性排除对象组，使其不受光源影响。有关更多信息，请参阅[层](Layers.html)。 |

## 详细信息

如果创建包含 Alpha 通道的纹理并将其分配给光源的 __Cookie__ 变量，则会从光源投射剪影。剪影的 Alpha 遮罩会调制光源亮度，从而在表面上产生亮点和暗点。是为场景增加复杂性或氛围的好方法。

Unity 中的所有[内置着色器](Built-inShaderGuide.html)能与任何类型的光源无缝协作。然而，[顶点光照 (VertexLit)](Built-inShaderGuide.html) 着色器无法显示剪影或阴影。

所有光源都可以选择性投射阴影。为此，应将每个光源的 __Shadow Type__ 属性设置为 __Hard Shadows__ 或 __Soft Shadows__。请参阅关于[阴影](ShadowOverview.html)的文档以了解更多信息。

## 方向光阴影

请参阅关于[方向光阴影](DirLightShadows.html)的文档以获取其工作原理的深入说明。请注意，使用前向渲染时，会对带有剪影的方向光禁用阴影。在此情况下，可以编写自定义着色器来启用阴影；请参阅关于[编写表面着色器](SL-SurfaceShaders.html)的文档以了解更多详细信息。

## 提示

* 带有剪影的__聚光灯__可以非常有效地产生光线从窗户进入的效果。
* 低强度点光源有助于为场景提供深度。
* 为了达到最大性能，请使用[顶点光照 (VertexLit)](Built-inShaderGuide.html) 着色器。此着色器仅执行顶点光照，从而在低端显卡上提供更高的吞吐量。

---

* <span class="page-edit"> 2017-06-08  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.6 版更新</span>
