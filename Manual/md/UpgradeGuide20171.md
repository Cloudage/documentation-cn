#升级到 Unity 2017.1


本页面列出了从早期版本的 Unity 升级到 2017.1 版时可能对现有项目造成影响的更改。

例如：

* 数据格式的变化可能导致需要重新烘焙。

* 任何现有函数、参数或组件值的含义或行为的变化。

* 任何函数或功能的弃用。（建议的替代方案。）

* * *

**UnityWebRequestTexture.GetTexture() nonReadable 参数更改**
 
这个便利 API 以前有一个错误，即 nonReadable 以相反的方式工作：将其设置为 true 将导致纹理可读，反之亦然。现在此问题已更正，参数按照文档所述的方式工作。请注意，如果直接创建 DownloadHandlerTexture，则不会受到影响。
 
***

 
**粒子系统 Stretched Billboard 轴点参数更改**
 
现在，X 轴的轴心在 Stretched Billboard 上是准确的。可能需要在受影响的项目中重新配置轴心设置。
 
Y 轴的轴心现在也是准确的，Unity 会在项目升级时自动重新配置这些轴心。
 

***


**着色器宏 UNITY_APPLY_DITHER_CROSSFADE 更改**

该宏用于在片元着色器中实现 LOD 交叉淡入淡出对象的纱门 (screendoor) 抖动效果。以前，需要将整个片元 IN 结构传递给该宏。现在只需将屏幕位置向量传递给它。

请记住，从 2017.1 开始，为 `#pragma surface` 指令添加了新的 `dithercrossfade` 选项，可以自动生成抖动代码。


***


<br/> 
* <span class="page-edit"> 2018-04-12 Page amended with no [editorial review](DocumentationEditorialReview.html)
</span>
