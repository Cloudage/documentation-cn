#Graphics Settings

![](../uploads/Main/GraphicsSettings.png) 

### Scriptable RenderLoop settings

这是实验性设置，允许定义一系列命令来精确控制场景的渲染方式（而不是使用 Unity 提供的默认渲染管线）。有关此实验性功能的更多信息，请参阅 GitHub 上的“可编程渲染管线”(Scriptable Render Pipeline) 文档。

### Camera settings

这些属性控制各种渲染设置。

|**设置：** |**描述：** |
|:---|:---|
|**Transparancy Sort Mode**| Unity 中的渲染器按几个条件排序，例如图层编号或与摄像机的距离。Transparency Sort Mode 添加了按可渲染对象沿特定轴的距离对可渲染对象进行排序的功能。这通常仅适用于 2D 开发；例如，按高度或沿 Y 轴来对精灵进行排序。|
|Default| 根据摄像机模式将对象进行排序。 |
|Perspective| 根据透视图将对象进行排序。|
|Orthographic| 根据正交视图将对象进行排序。 |
|**Transparancy Sort Axis**| 使用此项可以设置自定义透明度排序模式 (Transparency Sort Mode)。|


### Tier settings


![PlayerSettings Inspector 窗口中显示的 Tier Settings](../uploads/Main/GraphicsSettings2.png)

使用这些设置，可以通过调整内置定义来对渲染和着色器编译进行特定于平台的调整。例如，可以使用此功能在高端 iOS 设备上启用级联阴影，但在低端设备上禁用级联阴影以便提高性能。层由 [Rendering.GraphicsTier](../ScriptReference/Rendering.GraphicsTier.html) 定义。

|**属性：** |**功能：** |
|:---|:---|
|__Standard Shader Quality__|选择 Standard Shader Quality。| 
|__Reflection Probes Box Projection__|指定是否应使用 Reflection Probes Box Projection。| 
|__Reflection Probes Blending__|指定是否应启用 Reflection Probes Blending。| 
|__Detail Normal Map__|指定是否应对细节法线贴图 (Detail Normal Map) 采样（如果已分配）。| 
|__Enable Semitransparent Shadows__|指定是否应启用 Semitransparent Shadows。| 
|__Cascaded Shadows__|指定是否应使用级联阴影贴图。|
|__Use HDR__|将此字段设置为 true 可为此层启用 HDR 渲染。将此字段设置为 false 可为此层禁用 HDR 渲染。另请参阅：[高动态范围渲染](HDR.html)|
|__HDR Mode__|要用于此层的摄像机 HDR 模式 (CameraHDRMode)。|
|__Rendering Path__|应使用的渲染路径。|

### Built-in shader settings


使用这些设置可指定在列出的每个渲染路径中使用哪个着色器进行光照通道计算。

|**着色器：** |**计算：** |
|:---|:---|
|__Deferred__|使用延迟光照时使用，请参阅[摄像机：渲染路径](class-Camera.html) | 
|__Deferred Reflection__|在延迟光照下使用延迟反射（即反射探针）时使用，请参阅[摄像机：渲染路径](class-Camera.html) | 
|__Screen Space shadows__|使用屏幕空间阴影时使用。| 
|__Legacy deferred__|使用旧版延迟光照时使用，请参阅[摄像机：渲染路径](class-Camera.html)。| 
|__Motion vectors__|使用旧版延迟光照时使用，请参阅[网格渲染器：运动矢量](class-MeshRenderer.html)。| 
|__Lens Flare__|使用镜头光晕 (Lens Flares) 时使用，请参阅[光晕](class-Flare.html)。| 
|__Light Halo__|使用光环 (Light Halos) 时使用，请参阅[光环](class-Halo.html)。| 

|**设置：** |**描述：** |
|:---|:---|
|__Built-in shader__（默认值）| 使用 Unity 的内置着色器进行计算。 |
|__Custom shader__| 使用自己的兼容着色器进行计算。这样可以对延迟渲染进行深度自定义。 |
|__No Support__| 禁用此计算。如果不使用延迟着色或光照，请使用此设置。这样可以节省构建的游戏数据文件中的一些空间。 |

### Always-included Shaders

指定将始终与项目一起存储（即使场景中没有任何对象实际使用这些着色器）的[着色器](class-Shader.html)列表。应将流式 AssetBundle 使用的着色器添加到此列表中来确保可以访问这些着色器，这一点非常重要。

### Shader stripping - Lightmap modes 和 Fog modes

通过剥离涉及光照和雾效的某些着色器，降低构建数据大小并缩短加载时间。

|**_设置：_** |**_描述：_** |
|:---|:---|
|__Automatic__（默认值）| Unity 会查看场景和光照贴图设置来确定未使用的雾效和光照贴图模式，并跳过对应的着色器变体。 |
|__Manual__| 指定自己使用的模式。如果要在运行时通过脚本来[构建资源包](AssetBundles-Building.html)或更改雾模式，请选择此选项，确保包含了要使用的模式。 |

### Shader stripping - Instancing variants

|**_设置：_** |**_描述：_** |
|:---|:---|
|__Strip Unused__（默认值）| 构建项目时，仅当至少有一个引用着色器的材质勾选了“Enable instancing”复选框，Unity 才会包含实例化着色器变体。Unity 会剥离勾选了“Enable instancing”的材质未引用的着色器。  |
|__Strip All__| 剥离所有实例化着色器变体，即使它们正在被使用。|
|__Keep All__| 保留所有实例化着色器变体，即使它们未被使用。|

有关实例化变体的更多信息，请参阅 [GPU 实例化](GPUInstancing.html)。

### Shader preloading

指定要在加载游戏时预加载的着色器变体集合资源的列表。此列表中指定的着色器变体会在应用程序的整个生命周期内加载。可使用此属性来预加载使用频率极高的着色器。有关详细信息，请参阅[优化着色器加载时间](OptimizingShaderLoadTime.html)页面。


## 另请参阅

* [优化着色器加载时间](OptimizingShaderLoadTime.html)
* [优化图形性能](OptimizingGraphicsPerformance.html)
* [着色器参考](SL-Shader.html)

----

* <span class="page-edit">17-05-08 Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
* <span class="page-history">更新了 Tier Setting 的描述</span><br/>
* <span class="page-history">5.6 版中添加了新功能</span><br/>

