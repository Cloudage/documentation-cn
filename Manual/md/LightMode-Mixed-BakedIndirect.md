# Baked Indirect 模式

__Baked Indirect__ 模式是由场景中所有__混合光源__共用的一种光照模式。要将 Mixed lighting 设置为 __Baked Indirect__，请打开 Lighting 窗口（菜单：__Window__ > __Lighting__ > __Settings__），单击 __Scene__ 选项卡，导航至 __Mixed Lighting__，然后将 __Lighting Mode__ 设置为 __Baked Indirect__。请参阅[混合光照](LightMode-Mixed.html)相关文档以了解有关此光照模式的更多信息，并参阅[光照模式](LightModes.html)相关文档以了解有关其他可用模式的更多信息。

对于设置为 __Baked Indirect__ 模式的光源，Unity 仅预先计算间接光照，不执行阴影预计算。阴影在__阴影距离 (Shadow Distance)__（菜单：__Edit__ > __Project Settings__ > __Quality__ > __Shadows__）内是完全实时的。换句话说，__Baked Indirect__ 光照行为类似于带有额外间接光照但超出__阴影距离__后没有阴影的[实时光源](LightMode-Realtime.html)。您可以使用[后期处理雾效](PostProcessing-Fog.html)之类的效果来遮挡超过该距离的缺失阴影。

适合使用 __Baked Indirect__ 模式的一个很好的例子是，您想要在与走廊相连的房间内构建一个室内射击游戏或冒险游戏。此情况下的观察距离是有限的，因此可见的一切对象通常都应该在__阴影距离__内。此模式对于构建有雾的室外场景也很有用，因为您可以使用雾来隐藏远处缺失的阴影。

## 阴影

下表显示了使用 __Baked Indirect__ 模式时静态和动态游戏对象如何投射和接受阴影：

| | __动态阴影接受者<br/>__从另一个静态或动态游戏对象接受阴影的动态游戏对象 |  | __静态阴影接受者<br/>__从另一个静态或动态游戏对象接受阴影的静态游戏对象 ||
|:---|:---|:---|:---|:---| 
| | 在阴影距离内 | 超出阴影距离 | 在阴影距离内 | 超出阴影距离 |
| __动态阴影投射物<br/>__投射阴影的动态游戏对象| 阴影贴图 | - | 阴影贴图 | - |
| __静态阴影投射物<br/>__投射阴影的静态游戏对象| 阴影贴图 | - | 阴影贴图 | - |

## Baked Indirect 模式的优缺点

鉴于 __Baked Indirect__ 模式的性能要求，该模式非常适合以中端 PC 和高端移动设备为目标的构建。__Baked Indirect__ 模式最主要的优缺点如下：

### 优点

* 提供与[实时光照](LightMode-Realtime.html)相同的视觉效果。
* 为静态和动态游戏对象的所有组合提供实时阴影。
* 提供间接光照。

### 缺点

* 相对于其他__混合__光照模式具有更高的性能要求，因为它会将投射阴影的静态游戏对象渲染到阴影贴图中。
* 不会渲染超出__阴影距离__的阴影。

---

* <span class="page-edit"> 2017-06-08  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 5.6 版中添加了“光照模式”</span>
