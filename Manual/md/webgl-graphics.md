#WebGL 图形

WebGL 是一种用于在 Web 浏览器中渲染图形的 API，基于 OpenGL ES 图形库的功能。WebGL 1.0 大致与 OpenGL ES 2.0 功能相匹配，而 WebGL 2.0 大致与 OpenGL ES 3.0 功能相匹配。

## 延迟渲染

如果 WebGL2.0 可用，Unity WebGL 仅支持[延迟渲染路径](RenderTech-DeferredShading.html)。在 WebGL1.0 上，Unity WebGL 运行时将回退到[前向渲染](RenderTech-ForwardRendering.html)。

## 全局光照

Unity WebGL 仅支持[烘焙 GI](GIIntro.html)。WebGL 当前不支持实时 GI。此外，仅支持非方向光照贴图。


## 线性渲染

采用 WebGL 2.0 时，Unity WebGL 仅支持[线性颜色空间渲染](LinearLighting.html)。线性颜色空间渲染不支持回退到 WebGL 1.0。要使用线性颜色空间渲染来构建 WebGL 播放器，必须在 __Player Settings__ > __Other Settings__ 中删除 WebGL 1.0 API；取消选中 __Automatic Graphics API__。

某些 Web 浏览器不支持 [sRGB DXT 纹理压缩](class-TextureImporterOverride.html)。因此在使用线性渲染时会降低渲染性能的质量，原因是所有 DXT 纹理都要在运行时解压缩。

## MovieTexture

WebGL 不支持使用 MovieTexture 类来播放视频。但是，您可以使用 HTML5 视频元素有效地在 WebGL 内容中回放视频。请下载此 [Asset Store 资源包](https://www.assetstore.unity3d.com/en/#!/content/38369)查看此操作方法的示例。

## WebGL 着色器代码限制

WebGL 1.0 规范对 GLSLS 着色器代码施加了一些限制，比许多 OpenGL ES 2.0 实现方案更严格。当您编写自己的着色器时，受此影响最大。

具体而言，WebGL 限制了哪些值可用于索引数组或矩阵：WebGL 仅允许使用常量表达式、循环索引或组合的方式进行动态索引。唯一的例外是顶点着色器中的 uniform 访问，这种情况可使用任何表达式进行索引。

此外，在控制结构方面也有限制。允许的唯一循环类型是计算 _for_ 循环，在这种情况下，初始化函数 (initializer) 将变量初始化为常量，更新 (update) 向变量添加常量或从变量中减去常量，而延续测试 (continuation test) 会将变量与常量进行比较。不允许使用不符合这些条件的 _for_ 循环以及 _while_ 循环。

## 字体渲染

与所有 Unity 平台一样，Unity WebGL 支持动态字体渲染。但是，无法访问用户机器上安装的字体，因此所使用的任何字体都必须包含在项目文件夹中（包括国际字符的任何后备字体或者是字体的粗体/斜体版本），并[设置为后备字体名称](class-Font.html)。

## 抗锯齿

WebGL 对于大多数（但不是全部）浏览器和 GPU 组合均支持抗锯齿功能。要使用抗锯齿功能，必须在 WebGL 平台的默认[质量设置 (Quality Setting)](class-QualitySettings.html) 中启用该功能。

请注意，在 WebGL1.0 上有几个限制：

* 在运行时切换质量设置不会启用或禁用抗锯齿 - 必须在播放器启动时加载的默认质量设置中进行此设置。

* 不同的多重采样级别（2x、4x 等）在 WebGL 中没有任何影响，开启或关闭都可以。

* 应用于摄像机的任何[后期处理效果](PostProcessingOverview.html)都会禁用内置的抗锯齿功能。

* HDR 与抗锯齿功能不兼容，因此请确保禁用“Allow HDR”摄像机选项。

WebGL2.0 没有这样的限制。

## 反射探针

WebGL 支持反射探针，但由于 WebGL 规范中存在有关渲染到特定 Mipmap 的限制，因此不支持平滑实时反射探针（因此实时反射探针将始终产生锐利的反射，这种情况下可能看起来分辨率非常低）。若要支持平滑实时反射探针，必须使用 WebGL 2.0。

## WebGL 2.0 支持

Unity 包含对 WebGL 2.0 API 的支持，因此为 Web 带来了 OpenGL ES 3.0 级的渲染功能。

默认情况下，Unity WebGL 构建可支持 WebGL 1.0 和 WebGL 2.0 API。可在 WebGL __Player Settings__ > __Other Settings__ 中对此进行配置；要执行此操作，请取消选中 __Automatic Graphics API__。

浏览器支持 WebGL 2.0 时，内容可获得以下方面的优势：标准着色器提供的更高质量、[GPU 实例化](GPUInstancing.html)支持、方向光照贴图支持、着色器代码中的索引和循环不受限制以及更出色的性能。

---

* <span class="page-edit"> 2017-06-19  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2017.2](https://docs.unity3d.com/2017.2/Documentation/Manual/30_search.html?q=newin20172) 中添加了适用于 WebGL 2.0 的线性渲染 <span class="search-words">NewIn20172</span></span>
