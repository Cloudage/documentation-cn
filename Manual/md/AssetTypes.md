# 常见资源类型

## 图像文件
Unity 支持最常见的图像文件类型，例如 BMP、TIF、TGA、JPG 和 PSD。如果将分层的 Photoshop (.psd) 文件保存到 Assets 文件夹中，Unity 会将它们导入为展平的图像。如需了解详细信息，请参阅[从 Photoshop 导入包含 Alpha 通道的图像](HOWTO-alphamaps.html)或者[将图像作为精灵导入](SpriteEditor.html)

## FBX 和模型文件
由于 Unity 支持 FBX 文件格式，因此可以从任何支持 FBX 的 3D 建模软件导入数据。Unity 也支持本机导入 SketchUp 文件。如需了解如何在从 3D 建模软件导出 FBX 文件时获得最佳结果，请参阅[优化 FBX 文件](HOWTO-importObject.html)。

**注意：**还可以使用原生格式（例如 .max、.blend、.mb 和 .ma）从最常见的 3D 建模软件中保存 3D 文件。Unity 在 Assets 文件夹中找到这些文件时，会通过回调 3D 建模软件的 FBX 导出插件来导入它们。但是，建议将它们导出为 FBX 格式

## 网格和动画
无论使用哪种 3D 建模软件，Unity 都会从每个文件中导入网格和动画。有关 Unity 支持的 3D 建模软件列表，请参阅[模型文件格式](3D-formats.html)。

网格文件不需要导入动画。如果需要使用动画，则可以从单个文件导入所有动画，或导入单独的文件，每个文件包含一个动画。有关导入动画的更多信息，请参阅[模型导入工作流程](ImportingModelFiles.html)。

## 音频文件
如果将未压缩的音频文件保存到 Assets 文件夹中，Unity 会根据指定的压缩设置来导入音频文件。有关详细信息，请参阅[导入音频文件](AudioFiles.html)。

## 其他资源类型
虽然在 Unity 中可以选择各种方式来压缩、修改或以其他方式处理资源，但在任何情况下，Unity 都不会修改原始的源文件。导入过程会读取源文件，并在内部创建一个可直接用于游戏的资源表示，与所选的导入设置相匹配。如果修改资源的导入设置，或者对 Asset 文件夹中的源文件进行更改，则会导致 Unity 再次重新导入资源以反映这些更改。

**注意**：*导入原生 3D 格式要求 3D 建模软件与 Unity 安装在同一台计算机上。这是因为 Unity 要使用 3D 建模软件的 FBX Exporter 插件来读取文件。或者，也可以直接从应用程序导出为 FBX 格式并保存到 Projects 文件夹中。*

<a name="Standard"></a> 
## 标准资源

Unity 附带多个__标准资源__。这些资源是大多数 Unity 客户广泛使用的资源集合。包括：2D、Cameras、Characters、CrossPlatformInput、Effects、Environment、ParticleSystems、Prototyping、Utility、Vehicles。

Unity 使用__资源包__将__标准资源__传入和传出项目。

***注意：***如果在安装 Unity 时选择不安装标准资源，可以访问 [Asset Store](https://assetstore.unity.com/packages/essentials/asset-packs/standard-assets-32351)，从其中[下载这些资源](AssetPackages.html#Standard)。

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

