# 通过脚本访问和修改材质参数

在检视面板中，您所看到的所有材质参数都可以通过脚本来访问，这可以允许您在运行时修改材质的行为，利用这一点您可以制作出基于材质属性的动画效果。

在游戏运行过程中，您可以动态修改材质的数值属性、更改颜色，以及更换纹理。在这种工作中最常用到的函数有这些：

函数名称|用途
-------------|-----------------
[SetColor](../ScriptReference/Material.SetColor.html)     |更改材质的颜色（例如反照率着色颜色）
[SetFloat](../ScriptReference/Material.SetFloat.html)     |设置浮点值（例如，法线贴图乘数） 
[SetInt](../ScriptReference/Material.SetInt.html)       |在材质中设置整数值
[SetTexture](../ScriptReference/Material.SetTexture.html)   |为材质分配新纹理

完整的材质处理函数列表可以参考 [Material 类脚本参考](../ScriptReference/Material.html)。

有一点需要注意的是，这些函数**只会设置当前着色器能够使用**的材质属性。这意味着，如果着色器不使用任何纹理，或者根本没有绑定任何着色器，则调用 [SetTexture](../ScriptReference/Material.SetTexture.html) 将无效。即使稍后设置了需要纹理的着色器，也是如此。因此，建议您在设置任何属性之前，先设置着色器。一旦完成设置后，您就可以在使用同一组纹理和属性值的着色器之间切换，所有设定的值都会得到保留。

这些函数的作用与所有*简单*着色器（如旧版着色器）以及*除标准着色器以外*的内置着色器（如粒子、精灵、UI 和无光照着色器）相同。但是，对于使用*标准着色器*的材质，在完全修改材质之前，您还必须了解一些其他要求。

## 使用标准着色器编写脚本的特殊要求

如果要在运行时修改材质，标准着色器有一些额外的要求，因为标准着色器在幕后实际上是由许多不同着色器合为一体的着色器。

这些不同类型的着色器称为[着色器变体](SL-MultipleProgramVariants.html)，可以把它考虑为所有着色器功能的组合，其中部分功能处于激活状态，其余则处于未激活状态。

例如，如果您为材质设置了一个[法线贴图](StandardShaderMaterialParameterNormalMap.html)，就会激活支持法线贴图的着色器*变体*。如果随后又设置了[高度贴图](StandardShaderMaterialParameterHeightMap.html)，就会激活同时支持法线贴图和高度贴图的着色器变体。

这是一种很好的机制，它意味着当您使用标准着色器时，如果某个材质中没有使用法线贴图，它就不会存在法线贴图的性能开销，因为您实际运行的着色器省略了这部分功能的代码。这也意味着，如果您从不使用某个功能组合（例如“高度贴图”(HeightMap) 和“发光”(Emissive) 的组合），则您的构建版本中将完全忽略掉该变体；实际上，您通常只会用到标准着色器中极少数的可能变体。

Unity 会阻止简单地在构建中包含所有可能的着色器变体的行为，因为这将是一个非常大的数字，可能有数万个！这个巨大的数字不仅因为材质检视面板中每种可能的功能组合方式极其繁多，还因为针对不同渲染方案（例如是否使用 HDR、光照贴图、GI、雾效等）的每种功能组合存在多种变体。囊括所有这些可能性将导致加载缓慢、内存消耗高，并增加构建文件尺寸和构建时间。

所以 Unity 的替代方案是：通过检查项目中使用的材质资源来追踪您使用过的着色器变体。无论在项目中加入了哪些标准着色器变体，这些变体都会包含在构建中。

这个方案在通过脚本访问标准着色器材质的情况下，存在两个问题。

### 1.您必须为所需的标准着色器变体启用正确的关键字

如果使用脚本来更改材质，而这种更改会导致材质使用标准着色器的另一种不同变体，则必须**使用 [EnableKeyword](../ScriptReference/Material.EnableKeyword.html) 函数启用该变体**。如果您开始使用一个并未被材质使用过的着色器功能，另一个额外的变体就会被启用起来。例如将法线贴图分配给先前没有此贴图的材质，或者将初始为零的发光级别设置为大于零的值。

用来启用标准着色器功能的关键字如下：

|关键字        | 功能|
|---------------|---------|
|\_NORMALMAP     | [法线贴图](StandardShaderMaterialParameterNormalMap.html)|
|\_ALPHATEST_ON  | [“镂空”透明度](StandardShaderMaterialParameterRenderingMode.html)渲染模式|
|\_ALPHABLEND_ON | [“淡化”透明度](StandardShaderMaterialParameterRenderingMode.html)渲染模式|
|\_ALPHAPREMULTIPLY_ON  | [“透明”透明度](StandardShaderMaterialParameterRenderingMode.html)渲染模式|
|\_EMISSION | [发射颜色](StandardShaderMaterialParameterEmission.html)或[发射贴图](StandardShaderMaterialParameterEmission.html)|
|\_PARALLAXMAP | [高度贴图](StandardShaderMaterialParameterHeightMap.html)|
|\_DETAIL_MULX2 | [辅助“细节”贴图](StandardShaderMaterialParameterDetail.html)（反照率和法线贴图）|
|\_METALLICGLOSSMAP | 金属性工作流程中的[金属性/平滑度贴图](StandardShaderMaterialParameterMetallic.html)|
|\_SPECGLOSSMAP | 镜面反射工作流程中的[镜面反射/平滑度贴图](StandardShaderMaterialParameterSpecular.html)|

使用上面的关键字足以让您的材质修改脚本在编辑器运行模式中起效。

但是，由于 Unity 仅通过检查项目中使用的材质来确定要在构建中包含哪些变体，因此它不会包含*仅*在运行时通过脚本使用到的变体。

这就是说，如果您在脚本中为材质启用 _PARALLAXMAP 关键字，但是项目中未使用与该功能组合匹配的材质，则视差贴图将无法在最终构建中生效（即使在编辑器中看起来正常）。这是因为构建过程将会忽略该变体，因为它看起来不是必需的。

### 2.您必须确保 Unity 在构建中包含所需的着色器变体

为此，您需要确保 Unity 知道您要使用该着色器变体，具体方法就是在资源中包含至少一个该类型的材质。该材质**必须应用在场景中**，或者也可以将其放入 [Resources 文件夹](LoadingResourcesatRuntime.html)，否则该材质处于未使用状态，Unity 仍会在构建中忽略它。

通过完成上述两个步骤，即可在运行时使用标准着色器修改材质。

如果您有兴趣了解有关着色器变体的详细信息以及如何构建自己的变体，请阅读[制作多个着色器程序变体的信息](SL-MultipleProgramVariants.html)。

