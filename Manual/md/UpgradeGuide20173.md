#升级到 Unity 2017.3

<!-- Change submissions: https://docs.google.com/document/d/1Vi8WlG3ysu1RHrkNWPQ_N2q9glmSI8wr33fm2VCWNQ8/edit --> 

本页面列出了从早期版本的 Unity 升级到 2017.3 版时可能对现有项目造成影响的更改。

例如：

* 数据格式的变化可能导致需要重新烘焙。

* 任何现有函数、参数或组件值的含义或行为的变化。

* 任何函数或功能的弃用。（建议的替代方案。）

***

**Enlighten 中的光照贴图强度和发光材质**

2017.2 版中引入的一个错误增加了使用具有发光材质的静态网格对象为场景生成光照贴图的强度。烘焙和实时发光材质都有该问题。现在已经在 2017.3 版中修复了这一问题，因此现在的强度与 2017.1 版相似。

此更改会影响所有在 2017.2 中构建后升级到 2017.3 或更高版本的项目。

***

**PassType.VertexLMRGBM 已弃用**

在 Unity 2017.3 中，着色器通道 **VertexLMRGB** 会被忽略。例如：
```
Tags { "LightMode" = "VertexLMRGBM" }
```

现在，应使用支持所有光照贴图编码类型的 DecodeLightmap 着色器函数来提供或更新 **VertexLM** 着色器通道。内置的移动端着色器现在也使用 DecodeLightmap 着色器函数。

在桌面平台上使用内置移动着色器（如 Mobile 或 VertexLit）的现有项目中，光照输出可能会发生变化。这是因为 RGBM 编码值的最大范围已从 [0, 8] 更改为 [0, 5]。

----

* <span class="page-edit">2017-12-04  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
