#Android 2D 纹理覆盖

本页面将详细介绍 Android 特有的__纹理覆盖 (Texture Overrides)__。可在[此处](class-TextureImporter.html)找到常规纹理覆盖的说明。

本页面包含的信息假定读者具备 DXT 和 ETC 纹理压缩、OpenGL ES 和 Android 开发的基本知识。

有关纹理压缩和 OpenGL ES 的更多信息，请访问以下 Wikipedia 页面：

* [DXT 纹理压缩 (DXT Texture compression)](https://en.wikipedia.org/wiki/S3_Texture_Compression#DXT1)
* [Ericsson 纹理压缩 (ETC, Ericsson Texture Compression)](https://en.wikipedia.org/wiki/Ericsson_Texture_Compression)
* [有损压缩 (Lossy compression)](https://en.wikipedia.org/wiki/Lossy_compression)
* [OpenGL ES](https://en.wikipedia.org/wiki/OpenGL_ES#OpenGL_ES_3.0)


![Android 2D 纹理覆盖设置](../uploads/Main/TextureImporterOverride.png)


|__纹理格式__ |内部表示|
|:---|:---|
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed DXT1__ |压缩 RGB 纹理。受到 NVIDIA Tegra 的支持。4 位/像素（256x256 纹理为 32 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Crunched DXT1__ |压缩 RGB 纹理。受到 NVIDIA Tegra 的支持。Crunch 是一种基于 DXT 纹理压缩的有损压缩格式。在运行时，纹理由 CPU 解压缩为 DXT1，然后上传到 GPU。与常规 DXT1 压缩相比，Crunch 产生较小的纹理，但质量较低。Crunch 纹理可能需要很长时间进行压缩，但在运行时的解压缩速度非常快。4 位/像素（输出大小取决于纹理；对于 256x256 纹理而言的起始大小为 1 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed DXT5__ |压缩 RGBA 纹理。受到 NVIDIA Tegra 的支持。8 位/像素（256x256 纹理为 64 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Crunched DXT5__ |压缩 RGBA 纹理。受到 NVIDIA Tegra 的支持。Crunch 是一种基于 DXT 纹理压缩的有损压缩格式。在运行时，纹理在 CPU 上解压缩为 DXT5，然后上传到 GPU。与常规 DXT5 压缩相比，使用 Crunch 压缩可产生明显更小的纹理，但质量较低。Crunch 纹理可能需要很长时间进行压缩，但在运行时的解压缩速度非常快。8 位/像素（输出大小取决于纹理；对于 256x256 纹理而言的起始大小为 1 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ETC 4 bits__ |压缩 RGB 纹理。这是 Unity 中 Android 项目的默认纹理格式。ETC_RGB4 是 OpenGL ES 2.0 的一部分，受到所有 OpenGL ES 2.0 GPU 的支持。它不支持 Alpha。4 位/像素（256x256 纹理为 32 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Crunched ETC__ |压缩 RGB 纹理。Crunch 是一种基于 ETC 纹理压缩的有损压缩格式。在运行时，纹理由 CPU 解压缩为 ETC_RGB4，然后上传到 GPU。与常规 ETC_RGB4 压缩相比，Crunch 产生较小的纹理，但质量较低。Crunch 纹理可能需要很长时间进行压缩，但在运行时的解压缩速度非常快。ETC_RGB4 是 OpenGL ES 2.0 的一部分，受到所有 OpenGL ES 2.0 GPU 的支持。它不支持 Alpha。4 位/像素（输出大小取决于纹理；对于 256x256 纹理而言的起始大小为 1 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ETC2 4 bits__ |压缩 RGB 纹理。ETC2 是 OpenGL ES 3.0 的一部分，受到所有 OpenGL ES 3.0 GPU 的支持。4 位/像素（256x256 纹理为 32 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB + 1-bit Alpha Compressed ETC2 4 bits__ |具有 1 位穿透 Alpha 的压缩 RGB 纹理。ETC2 是 OpenGL ES 3.0 的一部分，受到所有 OpenGL ES 3.0 GPU 的支持。4 位/像素（256x256 纹理为 32 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed ETC2 8 bits__ |压缩 RGBA 纹理。受到所有 OpenGL ES 3.0 GPU 的支持。（256x256 纹理为 64 KB） |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Crunched ETC2__ |压缩 RGBA 纹理。Crunch 是一种基于 ETC 纹理压缩的有损压缩格式。在运行时，纹理在 CPU 上解压缩为 ETC2_RGBA8，然后上传到 GPU。与常规 ETC2_RGBA8 压缩相比，使用 Crunch 压缩可产生明显更小的纹理，但质量较低。Crunch 纹理可能需要很长时间进行压缩，但在运行时的解压缩速度非常快。受到所有 OpenGL ES 3.0 GPU 的支持。8 位/像素（输出大小取决于纹理；对于 256x256 纹理而言的起始大小为 1 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed PVRTC 2 bits__ |压缩 RGB 纹理。受到 Imagination PowerVR GPU 的支持。2 位/像素（256x256 纹理为 16 KB） |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed PVRTC 2 bits__ |压缩 RGBA 纹理。受到 Imagination PowerVR GPU 的支持。2 位/像素（256x256 纹理为 16 KB） |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed PVRTC 4 bits__ |压缩 RGB 纹理。受到 Imagination PowerVR GPU 的支持。4 位/像素（256x256 纹理为 32 KB） |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed PVRTC 4 bits__ |压缩 RGBA 纹理。受到 Imagination PowerVR GPU 的支持。4 位/像素（256x256 纹理为 32 KB） |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ATC 4 bits__ |压缩 RGB 纹理。受到 Qualcomm Snapdragon 的支持。4 位/像素（256x256 纹理为 32 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed ATC 8 bits__ |压缩 RGBA 纹理。受到 Qualcomm Snapdragon 的支持。8 位/像素（256x256 纹理为 64 KB）。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ASTC 4x4 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ASTC 5x5 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ASTC 6x6 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ASTC 8x8 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ASTC 10x10 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB Compressed ASTC 12x12 block__ | 压缩 RGB 纹理。受到所有 OpenGL ES 3.2 和 OpenGL ES 3.1 + AEP GPU 以及一部分 OpenGL ES 3.0 GPU 的支持。此压缩类型使用固定的 128 位块大小，并根据像素块大小（4x4 到 12x12）可达 8 到 0.89 位/像素。压缩纹理的大小从 256x256 纹理（4x4 块，最高质量）的 64 KB 到 256x256 纹理（12x12 块，最高压缩率）的 7.6 KB 不等。|
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed ASTC 4x4 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed ASTC 5x5 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed ASTC 6x6 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed ASTC 8x8 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed ASTC 10x10 block__ <br/><br/>&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA Compressed ASTC 12x12 block__ | 压缩 RGBA 纹理。受到所有 OpenGL ES 3.2 和 OpenGL ES 3.1 + AEP GPU 以及一部分 OpenGL ES 3.0 GPU 的支持。此压缩类型使用固定的 128 位块大小，并根据像素块大小（4x4 到 12x12）可达 8 到 0.89 位/像素。压缩纹理的大小从 256x256 纹理（4x4 块，最高质量）的 64 KB 到 256x256 纹理（12x12 块，最高压缩率）的 7.6 KB 不等。|
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB 16 bit__ |65,000 种颜色，没有 Alpha。使用比压缩格式更多的内存，但可能更适合没有渐变的 UI 或清晰纹理。256x256 纹理为 128 KB。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGB 24 bit__ |真实色彩，但没有 Alpha。256x256 纹理为 192 KB。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Alpha 8 bit__ |高质量 Alpha 通道，但没有任何颜色。256x256 纹理为 64 KB。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA 16 bit__ |低质量真实色彩。具有 Alpha 通道的纹理的默认压缩格式。256x256 纹理为 128 KB。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__RGBA 32 bit__ |真实色彩，并有 Alpha - 这是具有 Alpha 的纹理的最高质量压缩格式。256x256 纹理为 256 KB。 |
|__Compression quality__ |选择 Fast 可获得最快的性能，选择 Best 可获得最佳图像质量，选择 Normal 可获得这两者之间的平衡。 |

如果安装应用程序的设备不支持已使用的纹理压缩格式，Unity 会在应用程序运行时将纹理解压缩为 RGBA 32 并与压缩纹理一起存储在设备的内存中。这将导致加载时间增加和内存使用量增加，因为 Unity 必须解压缩纹理并存储两个版本的纹理（压缩和未压缩）。这也会对渲染性能产生很大影响，尤其是在较旧较慢的设备上。

除非是面向特定硬件（如 Nvidia Tegra），否则请使用 ETC2 压缩。ETC2 支持带有和不带 Alpha 通道的纹理，并且受到所有 OpenGL ES 3 设备的支持。要避免软件解压缩，请在 Player Settings 窗口中将最低图形 API 设置为 OpenGL ES 3，方法是从图形 API 列表中删除 OpenGL ES 2 并将 Minimum API Level 设置为 18+。

![](../uploads/Main/PlayerSettingsWindow.png) 

若要以 OpenGL ES 2 和 OpenGL ES 3 设备为目标设备，可创建两个不同的 APK，方法是首先构建面向 OpenGL ES 3 的 APK（如上所示），然后通过从 PlayerSettings 窗口的 Graphics API 部分中删除 OpenGL ES 2 和 Vulcan 来构建面向 OpenGL ES 2 的 APK。然后，可将这两个 APK 发布到 Google Play 应用商店。当用户下载应用程序时，Google Play 应用商店会自动为其设备安装最相关的 APK。有关发布多个 APK 文件的更多信息，请参阅 Android 开发者文档的[发布多个 APK (Publishing Multiple APKs)](https://developer.android.com/google/play/publishing/multiple-apks.html) 和 [Google Play 应用商店 APK 过滤 (Google Play Store APK filtering)](https://developer.android.com/google/play/filters.html) 部分。

在构建面向 OpenGL ES 2 的 APK 时，可使用 ETC1 纹理压缩。

面向 OpenGL ES2 的 APK 的纹理压缩格式可采用 ETC1。Unity 可将 ETC1 用于带有 Alpha 的纹理，但前提是这些纹理放置在[图集](https://docs.unity3d.com/Manual/SpritePacker.html)上（通过指定打包标签）并且构建目标为 Android。若要选择这样做，请勾选纹理的“Compress using ETC1”复选框。在后台，Unity 会将生成的图集拆分为两个纹理，每个纹理都带有 Alpha 通道，然后将它们组合在渲染管线的最后部分中。

如果确实要在纹理中存储 Alpha 通道，请使用所有硬件供应商都支持的 RGBA16 位压缩格式。

---

* <span class="page-edit">2017-09-18 Page amended with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">Unity 2017.1 中的更新功能</span>

* <span class="page-history">在 Unity 2017.3 中更新了 Crunch 压缩格式</span>
