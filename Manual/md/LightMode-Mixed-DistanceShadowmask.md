# Distance Shadowmask

__Distance Shadowmask__是 __Shadowmask（阴影遮罩）__光照模式的一个版本。该模式由场景中的所有混合光源共用。要将 __Mixed lighting__ 设置为 __Distance Shadowmask__，请执行以下操作：

* 打开 Lighting 窗口（菜单：__Window__ > __Lighting__ > __Settings__），单击 __Scene__ 选项卡，导航至 __Mixed Lighting__，然后将 __Lighting Mode__ 设置为 __Shadowmask__。
* 接下来，打开 __Quality Settings__（菜单：__Edit__ > __Project Settings__ > __Quality__），导航至 __Shadowmask Mode__，然后将其设置为 __Distance Shadowmask__。

请参阅[混合光照](LightMode-Mixed.html)相关文档以了解有关此光照模式的更多信息，并参阅[光照模式](LightModes.html)相关文档以了解有关其他可用模式的更多信息。

阴影遮罩是一种纹理，它与相应的光照贴图共享相同的 UV 布局和分辨率。它存储每个纹理像素的最多 4 个光源的遮挡信息，因为纹理在当前 GPU 上限制为最多 4 个通道。

__Distance Shadowmask__ 模式是 __Shadowmask__ 光照模式的一个版本，可提供从静态游戏对象投射到动态游戏对象的高质量阴影。

在__阴影距离 (Shadow Distance)__（菜单：__Edit__ > __Project Settings__ > __Quality__ > __Shadows__）内，Unity 会将动态和静态游戏对象渲染到阴影贴图中，可让静态游戏对象将清晰的阴影投射到动态游戏对象上。基于此原因，__Distance Shadowmask__ 模式相对于 __Shadowmask__ 模式具有更高的性能要求，因为场景中的所有静态游戏对象都将渲染到阴影贴图中。


超出__阴影距离__：

* 静态游戏对象通过预先计算的阴影遮罩从其他静态游戏对象接受高分辨率阴影。
* 动态游戏对象通过[光照探针](LightProbes.html)从静态游戏对象接受低分辨率的阴影。

适合使用 __Distance Shadowmask__ 模式的一个很好的例子是，您想要构建一个开放的世界场景，其中的阴影延伸到地平线，复杂的静态网格在移动的角色上投射实时阴影。

## 阴影

下表显示了使用 __Distance Shadowmask__ 模式时静态和动态游戏对象如何投射和接受阴影：

| | __动态阴影接受者__<br/>从另一个静态或动态游戏对象接受阴影的动态游戏对象 |  | __静态阴影接受者__<br/>从另一个静态或动态游戏对象接受阴影的静态游戏对象||
|:---|:---|:---|:---|:---| 
| | 在阴影距离内 | 超出阴影距离 | 在阴影距离内 | 超出阴影距离 |
| __动态阴影投射物__<br/>投射阴影的动态游戏对象| 阴影贴图 | - | 阴影贴图 | - |
| __静态阴影投射物__<br/>投射阴影的静态游戏对象| 阴影贴图 | 光照探针 | 阴影贴图 | 阴影遮罩 |



## Distance Shadowmask 模式的优缺点

鉴于 __Distance Shadowmask__ 模式的性能要求，该模式非常适合以高端 PC 和最新款游戏主机为目标的构建。__Distance Shadowmask__ 模式最主要的优缺点如下：

### 优点

* 提供与[实时光照](LightMode-Realtime.html)相同的视觉效果。
* 提供从动态游戏对象投射到静态游戏对象上和从静态游戏对象投射到动态游戏对象上的实时阴影。
* 着色器中的一个纹理操作即可处理静态游戏对象之间的所有光照和阴影。
* 自动合成动态和静态阴影。
* 提供间接光照。

### 缺点

* 最多只允许 4 个重叠的光源体积（请参阅[混合光照](LightMode-Mixed.html)的“技术细节”部分下的文档以了解更多信息）。
* 增加了光照贴图纹理集的内存要求。
* 增加了阴影遮罩纹理的内存要求。
* 增加了性能要求，因为 Unity 会将光照和阴影从静态游戏对象渲染到阴影贴图中。

---

* <span class="page-edit"> 2017-09-18 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 5.6 版中添加了“光照模式”</span>
