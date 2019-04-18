#公告牌渲染器 (Billboard Renderer)

公告牌渲染器可从预先制作的资源（从 SpeedTree 导出）或自定义创建的文件（例如，在运行时使用脚本创建或从自定义编辑器中创建）渲染[公告牌资源 (BillboardAssets)](class-BillboardAsset.html)。有关创建公告牌资源的更多信息，请参阅[公告牌资源 (BillboardAssets)](class-BillboardAsset.html) 手册页和 [BillboardAsset](../ScriptReference/BillboardAsset.html) API 参考。

公告牌是一种以更简单的方式绘制场景远处的复杂 3D 网格的细节级别 (LOD) 方法。因为网格很远，所以它在屏幕上的大小很小并且它在摄像机视图中作为焦点的可能性很低，这意味着通常不需要绘制它的全部细节。


![](../uploads/Main/BillboardRenderer.png) 


|**属性：** |**功能：** |
|:---|:---|
| __Cast Shadows__| 如果启用此属性，阴影投射光源照在公告牌上时将产生阴影。|
|&nbsp;&nbsp;&nbsp;&nbsp; _On_ | 启用阴影。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Off_ | 禁用阴影。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Two Sided_ | 允许从公告牌的任一侧投射阴影（即，不考虑背面剔除）。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Shadows Only_ | 显示阴影，但不显示公告牌本身。 |
| __Receive Shadows__ | 选中此框可以在公告牌上投射阴影。 |
| __Motion Vectors__ | 选中此框允许将共告牌的运动矢量渲染到摄像机运动矢量纹理中。请参阅脚本 API 中的 [Renderer.motionVectors](../ScriptReference/Renderer-motionVectors.html) 以了解更多信息。 |
| __Billboard__ | 如果有预先制作的公告牌资源，请将其放在这里以便将其分配给此公告牌渲染器。 |
| __Light Probes__ | 如果启用此属性，并且场景中存在烘焙[光照探针](LightProbes.html)，则公告牌渲染器将使用插值光照探针实施光照。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Off_ | 禁用光照探针。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Blend Probes_ | 应用于公告牌的光照由一个插值光照探针进行解释。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Use Proxy Volume_ | 应用于公告牌渲染器的光照由插值光照探针的 3D 网格进行解释。 |
| __Reflection Probes__ | 如果启用此属性，并且场景中存在[反射探针](ReflectionProbes.html)，则会为此游戏对象拾取反射纹理，并将此纹理设置为内置的着色器 uniform 变量。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Off_ | 禁用反射探针。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Blend Probes_ | 应用于公告牌的反射由相邻的反射探针进行解释，不考虑天空盒。此属性通常用于“室内”游戏对象或场景隐蔽区域（如洞穴和隧道）的游戏对象，因为天空不可见，所以不会被公告牌反射。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Blend Probes and Skybox_ | 此属性的作用与 Blend Probes 类似，但也允许在混合中使用天空盒。此属性通常用于天空始终可见并能反射天空的露天游戏对象。 |
|&nbsp;&nbsp;&nbsp;&nbsp; _Simple_ | 启用反射探针，但当存在两个重叠的探针体积时，探针之间不发生混合。 |
