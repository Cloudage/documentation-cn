#特定于平台的覆盖的纹理压缩格式

虽然 Unity 支持许多常见的图像格式作为导入[纹理](class-TextureImporter.html)的源文件（例如 JPG、PNG、PSD 和 TGA），但在 3D 图形硬件（如显卡或移动设备）的实时渲染过程中不会使用这些格式。3D 图形硬件要求纹理以专门格式进行压缩，这些格式针对快速纹理采样进行了优化。各种不同的平台和设备都有自己不同的专有格式。

默认情况下，Unity Editor 会自动将纹理转换为最合适的格式，以匹配您选择的构建目标。只有转换后的纹理才会包含在您的构建中；源资源文件保留为原始格式，位于项目的 _Assets_ 文件夹中。但是，在大多数平台上，可选择许多不同的受支持的纹理压缩格式。Unity 为每个平台设置了一些默认格式，但在某些情况下，您可能希望覆盖默认值并为某些纹理选择不同的压缩格式（例如，如果使用纹理作为遮罩，只有一个通道，则可以选择使用 BC4 格式来节省空间，同时保持质量）。

要为每个平台应用自定义设置，请使用[纹理导入器 (Texture Importer)](class-TextureImporter.html) 设置默认选项，然后使用__特定于平台的覆盖 (Platform-specific overrides)__ 面板覆盖特定平台的默认设置。

![每个平台的默认内部纹理表示
](../uploads/Main/class-TextureImporterOverride.png)

下表显示了每个平台使用的默认格式。

| **平台** | **颜色模型** | **无** | **正常质量（默认设置）** | **高质量** | **低质量（更高性能）** |
|:---|:---|:---|:---|:---|:---| 
| Windows、Linux、macOS、PS4、XBox One| RGB | RGB 24 位 | RGB Compressed DXT1 | RGB(A) Compressed BC7 | RGB Compressed DXT1 |
| | RGBA | RGBA 32 位 | RGBA Compressed DXT5 | RGB(A) Compressed BC7 | RGBA Compressed DXT5 |
| | HDR | RGBA Half | RGB Compressed BC6H  | RGB Compressed BC6H  | RGB Compressed BC6H  |
| WebGL| RGB | RGB 24 位 | RGB Compressed DXT1 | RGB Compressed DXT1 | RGB Compressed DXT1 |
| | RGBA | RGBA 32 位 | RGBA Compressed DXT5 | RGBA Compressed DXT5 | RGBA Compressed DXT5 |
| Android（默认子目标）| RGB | RGB 24 位 | RGB Compressed ETC | RGB Compressed ETC | RGB Compressed ETC |
| | RGBA | RGBA 32 位 | RGBA Compressed ETC2 | RGBA Compressed ETC2 | RGBA Compressed ETC2 |
| iOS| RGB | RGB 24 位 | RGB Compressed PVRTC 4 位 | RGB Compressed PVRTC 4 位 | RGB Compressed PVRTC 2 位 |
| | RGBA | RGBA 32 位 | RGBA Compressed PVRTC 4 位 | RGBA Compressed PVRTC 4 位 | RGBA Compressed PVRTC 2 位 |
| tvOS| RGB | RGB 24 位 | RGB Compressed ASTC 6x6 块 | RGB Compressed ASTC 4x4 块 | RGB Compressed ASTC 8x8 块 |
| | RGBA | RGBA 32 位 | RGBA Compressed ASTC 6x6 块 | RGBA Compressed ASTC 4x4 块 | RGBA Compressed ASTC 8x8 块 |
| | RGBA | RGBA 32 位 | RGBA 16 位 | RGBA 16 位 | RGBA 16 位 |



### 所有支持的纹理压缩格式

下表显示了每个平台上可用的纹理压缩格式选项以及生成的压缩文件大小（基于 256 像素平方图像）。选择纹理压缩格式需要在文件大小和质量之间取得平衡；质量越高，文件越大。在下面的描述中，假定游戏纹理的最终文件大小为 256 x 256 像素。

使用目标平台不支持的纹理压缩格式时，纹理将解压缩为 RGBA 32 并与压缩纹理一起存储在内存中。发生这种情况时，解压缩纹理会损失时间，并因为存储两次而导致内存损失。此外，所有平台都有不同的硬件，并经优化后可最有效地处理特定压缩格式；因此，选择不兼容的格式会影响游戏的性能。下表显示了每种压缩格式支持的平台。

关于下表的注意事项：

* __RGB__ 是一种颜色模型，其中红色、绿色和蓝色通过各种方式叠加在一起以再现各种[颜色](https://en.wikipedia.org/wiki/Color)。

* __RGBA__ 是具有 Alpha 通道的 __RGB__ 版本，支持混合和不透明度更改。

* __Crunch 压缩__是一种基于 DXT 或 ETC 纹理压缩的有损压缩格式（意味着压缩过程中会丢失部分[数据](https://simple.wikipedia.org/wiki/Information)）。在运行时，纹理在 CPU 上解压缩为 DXT 或 ETC，然后上传到 GPU。Crunch 压缩有助于纹理在磁盘上使用尽可能少的空间并方便下载。Crunch 纹理可能需要很长时间进行压缩，但在运行时的解压缩速度非常快。

| **纹理压缩格式**| **描述** | **大小**<br/>（256x256 像素纹理） | **平台支持** |
|:---|:---|:---|:---| 
| RGB Compressed DXT1 | 压缩无符号标准化整数 RGB 纹理。 | 32KB（4 位/像素） | Windows、Linux、macOS、PS4、XBox One、Android（Nvidia Tegra 和 Intel Bay Trail）、WebGL <br/><br/>注意：对于不支持 sRGB DXT 的 Web 浏览器上的[线性渲染](LinearLighting.html)，纹理将在运行时解压缩到 RGBA32。|
| RGB Crunched DXT1| 与 RGB Compressed DXT1 类似，但使用 Crunch 压缩方式进行压缩。有关 Crunch 压缩的更多信息，请参阅上面的注意事项。 | 可变，取决于纹理中内容的复杂程度。 | Windows、Linux、macOS、PS4、XBox One、Android（Nvidia Tegra 和 Intel Bay Trail）、WebGL <br/><br/>注意：对于不支持 sRGB DXT 的 Web 浏览器上的[线性渲染](LinearLighting.html)，纹理将在运行时解压缩到 RGBA32。|
| RGBA Compressed DXT5| 压缩无符号标准化整数 RGBA 纹理。8 位/像素。 | 64KB（8 位/像素） | Windows、Linux、macOS、PS4、XBox One、Android（Nvidia Tegra 和 Intel Bay Trail）、WebGL <br/><br/>注意：对于不支持 sRGB DXT 的 Web 浏览器上的[线性渲染](LinearLighting.html)，纹理将在运行时解压缩到 RGBA32。|
| RGBA Crunched DXT5  | 与 RGBA Compressed DXT5 类似，但使用 Crunch 压缩方式进行压缩。有关 Crunch 压缩的更多信息，请参阅上面的注意事项。 | 可变，取决于纹理中内容的复杂程度。 | Windows、Linux、macOS、PS4、XBox One、Android（Nvidia Tegra 和 Intel Bay Trail）、WebGL <br/><br/>注意：对于不支持 sRGB DXT 的 Web 浏览器上的[线性渲染](LinearLighting.html)，纹理将在运行时解压缩到 RGBA32。|
| RGB Compressed BC6H | 压缩无符号浮点/高动态范围 (HDR) RGB 纹理。  | 64KB（8 位/像素） | Windows Direct3D 11：OpenGL 4、Linux。<br/><br/>注意：在以下平台配置中，BC6H 纹理在运行时解压缩到 RGBA half：<br/>- 搭载 OpenGL 的 macOS<br/>- 搭载 Direct3D 10 着色器模型 4 或 OpenGL 3 GPU 的平台。  |
| RGB(A) Compressed BC7| 高质量压缩无符号标准化整数 RGB 或 RGBA 纹理。  | 64KB（8 位/像素） | Windows Direct3D 11：OpenGL 4、Linux <br/><br/>注意：在以下平台配置中，BC7 纹理在运行时解压缩到 RGBA 32 位：<br/>- 搭载 OpenGL 的 macOS<br/>- 搭载 Direct3D 10 着色器模型 4 的平台<br/>搭载 OpenGL 3 GPU 的平台。 |
| RGB Compressed ETC| 压缩 RGB 纹理。这是适用于 Android 项目的不带 Alpha 通道的纹理的默认纹理压缩格式。 | 32KB（4 位/像素） | Android、iOS、tvOS。<br/>注意：ETC1 受到所有 OpenGL ES 2.0 GPU 的支持。它不支持 Alpha。 |
| RGB Crunched ETC| 与 RGB Compressed ETC 类似，但使用 Crunch 压缩方式进行压缩。有关 Crunch 压缩的更多信息，请参阅上面的注意事项。 | 可变，取决于纹理中内容的复杂程度。 | Android、iOS、tvOS。|
| RGB Compressed ETC2| 压缩 RGB 纹理。 | 32KB（4 位/像素） | Android (OpenGL ES 3.0) <br/>注意：在不支持 ETC2 的 Android 平台上，纹理在运行时解压缩为 Build Settings 中的 __ETC2 fallback__ 指定的格式。 |
| RGBA Compressed ETC2 | 压缩 RGBA 纹理。这是适用于 Android 项目的带有 Alpha 通道的纹理的默认纹理压缩格式。 | 64KB（8 位/像素） | Android (OpenGL ES 3.0)、iOS (OpenGL ES 3.0)、tvOS (OpenGL ES 3.0) <br/><br/>注意：在不支持 ETC2 的 iOS 和 tvOS 设备上，纹理在运行时解压缩为 RGBA32。在不支持 ETC2 的 Android 平台上，纹理在运行时解压缩为 Build Settings 中的 __ETC2 fallback__ 指定的格式。|
| RGBA Crunched ETC2 | 与 RGBA Compressed ETC2 类似，但使用 Crunch 压缩方式进行压缩。有关 Crunch 压缩的更多信息，请参阅上面的注意事项。 | 可变，取决于纹理中内容的复杂程度。 | Android (OpenGL ES 3.0)、iOS (OpenGL ES 3.0)、tvOS (OpenGL ES 3.0) <br/><br/>注意：在不支持 ETC2 的 iOS 和 tvOS 设备上，纹理在运行时解压缩为 RGBA32。在不支持 ETC2 的 Android 平台上，纹理在运行时解压缩为 Build Settings 中的 __ETC2 fallback__ 指定的格式。 |
| RGB Compressed ASTC| 可变块大小压缩 RGB 纹理。 | 12x12：0.89 位/像素（256x256 纹理的大小为 7.56KB）<br/>10x10：1.28 位/像素（256x256 纹理的大小为 10.56KB）<br/>8x8：2 位/像素（256x256 纹理的大小为 16KB）；<br/>6x6：3.56 位/像素（256x256 纹理的大小为 28.89KB）<br/>5x5：5.12 位/像素（256x256 纹理的大小为 42.25KB）<br/>4x4：8 位/像素（256x256 纹理的大小为 64KB） | tvOS（所有）、iOS (A8)、Android（PowerVR 6XT、Mali T600 系列、Adreno 400 系列、Tegra K1）。 |
| RGBA Compressed ASTC| 可变块大小压缩 RGBA 纹理。 | 12x12：0.89 位/像素（256x256 纹理的大小为 7744 字节）<br/>10x10：1.28 位/像素（256x256 纹理的大小为 10816 字节）<br/>8x8：2 位/像素（256x256 纹理的大小为 16KB）；<br/>6x6：3.56 位/像素（256x256 纹理的大小为 29584 字节）<br/>5x5：5.12 位/像素（256x256 纹理的大小为 43264 字节）<br/>4x4：8 位/像素（256x256 纹理的大小为 64KB） | tvOS（所有）、iOS (A8)、Android（PowerVR 6XT、Mali T600 系列、Adreno 400 系列、Tegra K1）。 |
| RGB Compressed PVRTC 2 位| 高压缩 RGB 纹理。质量低，但较小，因此提高了性能。 | 16KB（2 位/像素） | Android (PowerVR)、iOS、tvOS。 |
| RGBA Compressed PVRTC 2 位| 高压缩 RGBA 纹理。质量低，但较小，因此提高了性能。 | 16KB（2 位/像素） | Android (PowerVR)、iOS、tvOS。 |
| RGB Compressed PVRTC 4 位| 压缩 RGB 纹理。高质量纹理，尤其是颜色数据，但可能需要很长时间压缩。 | 32KB（4 位/像素） | Android (PowerVR)、iOS、tvOS。 |
| RGBA Compressed PVRTC 4 位| 压缩 RGB 纹理。高质量纹理，尤其是颜色数据，但可能需要很长时间压缩。 | 32KB（4 位/像素） | Android (PowerVR)、iOS、tvOS。 |
| RGB Compressed ATC| 压缩 RGB 纹理。 | 32KB（4 位/像素） | Android (Qualcomm - Adreno)、iOS、tvOS。 |
| RGBA Compressed ATC| 压缩 RGBA 纹理。 | 64KB（8 位/像素） | Android (Qualcomm - Adreno)、iOS、tvOS。 |
| RGB 16 位| 65,000 种颜色，没有 Alpha。使用比压缩格式更多的内存，但可能更适合没有渐变的 UI 或清晰纹理。 | 128KB（16 位/像素） | 所有平台。 |
| RGB 24 位| 真实色彩，但没有 Alpha。  | 192KB（24 位/像素） | 所有平台 |
| Alpha 8| 高质量 Alpha 通道，但没有任何颜色。  | 64KB（8 位/像素） | 所有平台。 |
| RGBA 16 位| 低质量真实色彩。这是具有 Alpha 通道的纹理的默认压缩格式。  | 128KB（16 位/像素） | 所有平台。 |
| RGBA 32 位| 真实色彩，并有 Alpha。这是具有 Alpha 通道的纹理的最高质量压缩格式。 | 256KB（32 位/像素） | 所有平台。 |

可从 DDS 文件导入纹理，但仅支持 DXT、BC 压缩格式或未压缩像素格式。

### 关于 Android 的注意事项

除非针对特定硬件（例如 Tegra），否则 ETC2 压缩是 Android 最高效的选项，可提供最佳的质量与文件大小（及相关的内存大小要求）平衡。如果需要 Alpha 通道，可将其存储在外部，并仍能受益于较低的纹理文件大小。

可对具有 [Alpha 通道](HOWTO-alphamaps.html)的纹理使用 ETC1 格式，但仅当构建目标为 Android 且纹理放置在图集上（通过指定打包标签）时才适用。要启用此功能，请勾选纹理的 __Compress using ETC1__ 复选框。Unity 会将生成的图集拆分为两个纹理，每个纹理都没有 Alpha 通道，然后将它们组合在渲染管线的最后部分中。

要在纹理中存储 Alpha 通道，请使用 RGBA16 位压缩，这是所有硬件供应商都支持的格式。

---

* <span class="page-edit">2017-11-20  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2017.2](https://docs.unity3d.com/2017.2/Documentation/Manual/30_search.html?q=newin20172) 中添加了适用于 WebGL 的线性渲染 <span class="search-words">NewIn20172</span></span>

* <span class="page-history">在 [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 版中更新了 Crunch 压缩格式 <span class="search-words">NewIn20173</span></span>

* <span class="page-history">在 [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 中停止了 Tizen 和三星电视 (Samsung TV) 支持 <span class="search-words">NewIn20173</span></span>
