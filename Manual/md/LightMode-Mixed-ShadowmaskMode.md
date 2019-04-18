#Shadowmask 模式

Shadowmask 模式是由场景中所有混合光源共用的一种光照模式。要将 Mixed lighting 设置为 Shadowmask，请打开 Lighting 窗口（菜单：__Window__ > __Lighting__ > __Settings__），单击 __Scene__ 选项卡，导航至 __Mixed Lighting__，然后将 __Lighting Mode__ 设置为 __Shadowmask__。

此外，还需要从 __Quality Settings__ 中选择所需的 Shadowmask 模式（菜单：__Edit__ > __Project Settings__ > __Quality__）：

* [Shadowmask](LightMode-Mixed-Shadowmask.html)：投射阴影的静态游戏对象总是使用烘焙阴影。
* [Distance Shadowmask](LightMode-Mixed-DistanceShadowmask.html)：Unity 使用在__阴影距离 (Shadow Distance)__ 内使用实时阴影，而在超出此距离后使用烘焙阴影。

请参阅[混合光照](LightMode-Mixed.html)和[光照模式](LightModes.html)相关文档以了解有关其他可用模式的更多信息。

---

* <span class="page-edit">2017-09-18  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2017.1](https://docs.unity3d.com/2017.1/Documentation/Manual/30_search.html?q=newin20171) 版中向 Quality Settings 添加了 Shadowmask 模式 <span class="search-words">NewIn20171</span></span>
