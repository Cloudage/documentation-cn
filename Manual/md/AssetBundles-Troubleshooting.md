# 故障排除

本部分将介绍使用 AssetBundle 的项目中常见的几个问题。

##资源重复
当对象构建到 AssetBundle 中时，Unity 5 的 AssetBundle 系统会查找对象的所有依赖项。这是使用资源数据库完成的。此依赖关系信息用于确定包含在 AssetBundle 中的对象集。

显式分配给 AssetBundle 的对象将仅构建到该 AssetBundle 中。当对象的 AssetImporter 将其 assetBundleName 属性设置为非空字符串时，表示“显式指定”该对象。

未显式分配到 AssetBundle 中的任何对象将包含在所有 AssetBundle 中，这些 AssetBundle 会包含一个或多个引用该未标记对象的对象。

如果将两个不同的对象分配给两个不同的 AssetBundle，但两者都引用了一个共同的依赖项对象，那么该依赖项对象将被复制到两个 AssetBundle 中。复制的依赖项也将被实例化，这意味着依赖项对象的两个副本将被视为具有不同标识符的不同对象。这将增加应用程序的 AssetBundle 的总大小。如果应用程序加载对象的两个父项，则还会导致将两个不同的对象副本加载到内存中。

有几种方法可以解决这个问题：

1.确保构建到不同 AssetBundle 中的对象不共享依赖项。任何共享依赖项的对象都可以放在同一个 AssetBundle 中，而不会复制它们的依赖项。

    * 对于具有许多共享依赖项的项目，此方法通常不可行。这种情况下可能生成单独的 AssetBundle，必须高度频繁进行重建和重新下载，因此很不方便或高效。

2.对 AssetBundle 进行分段，确保不会同时加载两个共享依赖项的 AssetBundle。

    * 此方法可能适用于某些类型的项目，例如基于关卡的游戏。但是，仍然会不必要地增加项目的 AssetBundle 大小，并增加构建时间和加载时间。

3.确保所有依赖项资源都构建到自己的 AssetBundle 中。这样可以完全消除重复资源的风险，但也带来了复杂性。应用程序必须跟踪 AssetBundle 之间的依赖关系，并确保在调用任何 AssetBundle.LoadAsset API 之前加载了正确的 AssetBundle。

Unity 5 通过位于 UnityEditor 命名空间中的 AssetDatabase API 来跟踪对象依赖项。正如命名空间的名称所示，此 API 仅在 Unity Editor 中可用，而不能在运行时使用。可使用 AssetDatabase.GetDependencies 查找特定对象或资源的所有直接依赖项。请注意，这些依赖项可能还有自己的依赖项。此外，可使用 AssetImporter API 查询分配了任何特定对象的 AssetBundle。

通过组合 AssetDatabase 和 AssetImporter API，可以编写一个 Editor 脚本，确保将所有 AssetBundle 的直接或间接依赖项都分配给 AssetBundle，或者不会有两个 AssetBundle 共享尚未分配给 AssetBundle 的依赖项。由于复制资源的内存成本，建议所有项目都采用这样的脚本。

##精灵图集重复
以下部分将介绍 Unity 5 的资源依赖性计算代码在与自动生成的精灵图集结合使用时出现的奇怪行为。

任何自动生成的精灵图集都将与生成精灵图集的精灵对象一起分配到同一个 AssetBundle。如果精灵对象被分配给多个 AssetBundle，则精灵图集将不会被分配给 AssetBundle 并且将被复制。如果精灵对象未分配给 AssetBundle，则精灵图集也不会分配给 AssetBundle。

为了确保精灵图集不重复，请确保标记到相同精灵图集的所有精灵都被分配到同一个 AssetBundle。

Unity 5.2.2p3 及更低版本

自动生成的精灵图集永远不会分配给 AssetBundle。因此，它们将被包含在任何包含其组成精灵的 AssetBundle 中，还会包含在任何引用其组成精灵的 AssetBundle 中。

由于这个问题，强烈建议将使用 Unity Sprite Packer 的所有 Unity 5 项目都升级到 Unity 5.2.2p4、5.3 或任何更高版本的 Unity。

对于无法升级的项目，此问题有两种解决方法：

1.简单：避免使用 Unity 的内置 Sprite Packer。外部工具生成的精灵图集将是正常的资源，可以正确分配给 AssetBundle。

2.困难：将所有使用自动加入图集的精灵的对象分配给与精灵相同的 AssetBundle。

    * 这将确保生成的精灵图集不被视为任何其他 AssetBundle 的间接依赖项，并且不会被复制。

    * 此解决方案保留了使用 Unity Sprite Packer 的工作流程，但会降低开发人员将资源分离到不同 AssetBundle 的能力，并会强制在引用图集的任何组件上的任何数据发生变化时重新下载整个精灵图集，即使该图集本身没有变化也是如此。

##Android 纹理
由于 Android 生态系统中存在严重的设备碎片，因此通常需要将纹理压缩为多种不同的格式。虽然所有 Android 设备都支持 ETC1，但 ETC1 不支持具有 Alpha 通道的纹理。如果应用程序不需要 OpenGL ES 2 支持，解决该问题的最简单方法是使用所有 Android OpenGL ES 3 设备都支持的 ETC2。

大多数应用程序需要在不支持 ETC2 的旧设备上发布。解决这个问题的一种方法是使用 Unity 5 的 AssetBundle 变体。（有关其他方案的详细信息，请参阅 Unity 的 Android 优化指南。）

要使用 AssetBundle 变体，必须将无法使用 ETC1 完全压缩的所有纹理隔离到仅包含纹理的 AssetBundle 中。接下来，使用供应商特有的纹理压缩格式（如 DXT5、PVRTC 和 ATITC），创建这些 AssetBundle 的足够多变体来支持 Android 生态系统中不支持 ETC2 的部分。对于每个 AssetBundle 变体，应将包含的纹理的 TextureImporter 设置更改为适合变体的压缩格式。

在运行时，可以使用 [SystemInfo.SupportsTextureFormat](http://docs.unity3d.com/ScriptReference/SystemInfo.SupportsTextureFormat.html?_ga=1.141687282.1751468213.1479139860) API 检测对不同纹理压缩格式的支持情况。应使用此信息来选择和加载含有以受支持格式压缩的纹理的 AssetBundle 变体。

有关 Android 纹理压缩格式的更多信息，请查看[此处](http://developer.android.com/guide/topics/graphics/opengl.html#textures)。

##iOS 文件句柄过度使用
Unity 5.3.2p2 中修复了以下部分中描述的问题。最新版本的 Unity 不受此问题的影响。

在 Unity 5.3.2p2 之前的版本中，Unity 将在加载 AssetBundle 的整个时间内保留 AssetBundle 的打开文件句柄。这在大多数平台上都不是问题。但是，iOS 将一个进程可以同时打开的文件句柄数量限制为 255。如果加载 AssetBundle 导致超出此限制，则加载调用将失败，并显示“Too Many Open File Handles”错误。

对于试图将内容划分为数百或数千个 AssetBundle 的项目而言，这是一个常见问题。

对于无法升级到修补版 Unity 的项目来说，临时解决方案如下：

* 通过合并相关的 AssetBundle 减少正在使用的 AssetBundle 数量
* 使用 AssetBundle.Unload(false) 关闭 AssetBundle 的文件句柄，并手动管理加载的对象的生命周期


<br/> 

---
* <span class="page-edit">2017-05-15  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>


