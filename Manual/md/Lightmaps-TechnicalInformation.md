# 光照贴图：技术信息

Unity 可存储采用不同压缩和编码方案的光照贴图，具体取决于目标平台以及 Lighting 窗口中的压缩设置。

##编码方案

Unity 项目可以使用两种技术在必要时将烘焙光源强度范围编码为低动态范围纹理：

* **RGBM 编码**。RGBM 编码将颜色存储在 **RGB** 通道中，将乘数 (**M**) 存储在 Alpha 通道中。在线性空间中，RGBM 光照贴图的范围是 0 到 34.49(5<sup>2.2</sup>)，而在伽马空间中，范围是 0 到 5。

* **双低动态范围 (dLDR) 编码**。只需直接将 [0, 2] 范围映射到 [0, 1]，即可在移动平台上使用 dLDR 编码。值高于 2 的烘焙光源强度将被钳制。编码值的计算方法是：光照贴图纹理的值乘以 2（如果使用伽马空间），或乘以 4.59482(2<sup>2.2</sup>)（如果使用线性空间）。某些平台将光照贴图存储为 dLDR，这是因为在使用 RGBM 时，这些平台的硬件压缩会产生外观不佳的瑕疵。

使用线性颜色空间时，光照贴图纹理将标记为 sRGB，着色器使用的最终值（采样和解码后）将位于线性颜色空间中。使用伽马颜色空间时，最终值将在伽马颜色空间中。

**注意**：使用编码时，存储在光照贴图（GPU 纹理内存）中的值始终位于伽马颜色空间中。

_UnityCG.cginc_ 着色器 include 文件中的 __Decode Lightmap__ 着色器函数负责在从着色器中的光照贴图纹理读取值之后解码光照贴图值。

##HDR 光照贴图支持

HDR 光照贴图可用于 PC、Mac 和 Linux 独立平台、Xbox One 和 PlayStation 4。Player Settings 检视面板为这些平台提供了 **Lightmap Encoding** 选项，用于控制光照贴图的编码/压缩。

![](../uploads/Main/LightmapsTechnicalDetails.png) 

选择 __High Quality__ 将启用 HDR 光照贴图支持，而 __Normal Quality__ 将切换为使用 __RGBM__ 编码。

在 Lighting 窗口中启用了光照贴图 __Compression__ 时，将使用 __BC6H__ 压缩格式来压缩光照贴图。

##高质量 (BC6H) 光照贴图的优势

* HDR 光照贴图不使用任何编码方案来编码光照贴图值，因此支持的范围仅受到 16 位浮点纹理格式的限制（范围从 0 到 65504）。

* BC6H 格式质量优于 DXT5 + RGBM 格式编码，并且不会产生 RGBM 编码所具有的任何色带瑕疵。

* 需要对 HDR 光照贴图进行采样的着色器是一些较短的 ALU 指令，因为不需要对采样值进行解码。

* BC6H 格式与 DXT5 具有相同的 GPU 内存要求。

以下列表列出了每个目标平台的编码方案及其纹理压缩格式：

| **目标平台** | **编码** | **压缩 - 大小（每像素位数）** |
|:--|:--|:--|
| 独立平台（PC、Mac 和 Linux） | RGBM / HDR | DXT5 / BC6H - 8 bpp |
| Xbox One | RGBM / HDR | DXT5 / BC6H - 8 bpp |
| PlayStation4 | RGBM / HDR | DXT5 / BC6H - 8 bpp |
| WebGL 1.0 / 2.0 | RGBM | DXT5 - 8 bpp |
| iOS | dLDR | PVRTC RGB - 4 bpp |
| tvOS | dLDR | ASTC 4x4 块 RGB - 8 bpp |
| Android* | dLDR | ETC1 RGB - 4 bpp |
| 三星电视 | dLDR | ETC1 RGB - 4 bpp |
| Nintendo 3DS | dLDR | ETC1 RGB - 4 bpp |
| Nintendo 3DS | dLDR | ETC1 RGB - 4 bpp |

*如果目标平台是 __Android__，则可将 __Build Settings__ 中的默认纹理压缩格式改写为以下格式之一：DXT1、PVRTC、ATC、ETC2 或 ASTC。默认格式为 ETC。

##预计算实时全局光照 (GI)

GI 系统的输入与输出相比有不同的范围和编码。在伽马空间中，表面反照率为 8 位无符号整数 RGB，在线性空间中，发光为 16 位浮点 RGB。有关使用 Meta pass 提供自定义输入的建议，请参阅有关 [Meta pass](MetaPass.html) 的文档。

辐照度输出纹理将以 RGB9E5 共享指数浮点格式（如果图形硬件支持此格式）存储，或者以范围在 5 以内的 RGBM 作为后备格式。RGB9E5 光照贴图的范围是 [0, 65408]。有关 RGB9E5 格式的详细信息，请参阅 [Khronos.org：EXT_texture_shared_exponent](https://www.khronos.org/registry/OpenGL/extensions/EXT/EXT_texture_shared_exponent.txt)。

另请参阅：

* [纹理导入器覆盖](class-TextureImporterOverride.html)
* [纹理类型](TextureTypes.html)
* [全局光照](GlobalIllumination.html)
* [纹理类型](TextureTypes.html)
* [全局光照](GlobalIllumination.html)

---

* <span class="page-edit">2017-09-18  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 Unity [2017.2](https://docs.unity3d.com/2017.2/Documentation/Manual/30_search.html?q=newin20172) 中添加了烘焙光照贴图 <span class="search-words">NewIn20172</span></span>

* <span class="page-history">在 Unity [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 中添加了 HDR 光照贴图支持 <span class="search-words">NewIn20173</span></span>
