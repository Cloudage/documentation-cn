# 构建 AssetBundle

在有关 [AssetBundle 工作流程](AssetBundles-Workflow.html)的文档中，我们有一个代码示例将三个参数传递给 `BuildPipeline.BuildAssetBundles` 函数。让我们深入了解一下这方面。

_Assets/AssetBundles_：这是 AssetBundle 要输出到的目录。可以将其更改为所需的任何输出目录，只需确保在尝试构建之前文件夹实际存在。

#### BuildAssetBundleOptions

可以指定几个具有各种效果的不同 `BuildAssetBundleOptions`。请参阅关于 [BuildAssetBundleOptions](../ScriptReference/BuildAssetBundleOptions.html) 的脚本 API 参考查看所有这些选项的表格。

虽然可以根据需求变化和需求出现而自由组合 `BuildAssetBundleOptions`，但有三个特定的 `BuildAssetBundleOptions` 可以处理 AssetBundle 压缩：

* `BuildAssetBundleOptions.None`：此捆绑包选项使用 LZMA 格式压缩，这是一个压缩的 LZMA 序列化数据文件流。LZMA 压缩要求在使用捆绑包之前对整个捆绑包进行解压缩。此压缩使文件大小尽可能小，但由于需要解压缩，加载时间略长。值得注意的是，在使用此 BuildAssetBundleOptions 时，为了使用捆绑包中的任何资源，必须首先解压缩整个捆绑包。<br/> 解压缩捆绑包后，将使用 LZ4 压缩技术在磁盘上重新压缩捆绑包，这不需要在使用捆绑包中的资源之前解压缩整个捆绑包。最好在包含资源时使用，这样，使用捆绑包中的一个资源意味着将加载所有资源。这种捆绑包的一些用例是打包角色或场景的所有资源。<br/>由于文件较小，建议仅从异地主机初次下载 AssetBundle 时才使用 LZMA 压缩。下载文件后，文件将被缓存为 LZ4 压缩包。

* `BuildAssetBundleOptions.UncompressedAssetBundle`：此捆绑包选项采用使数据完全未压缩的方式构建捆绑包。未压缩的缺点是文件下载大小增大。但是，下载后的加载时间会快得多。

* `BuildAssetBundleOptions.ChunkBasedCompression`：此捆绑包选项使用称为 LZ4 的压缩方法，因此压缩文件大小比 LZMA 更大，但不像 LZMA 那样需要解压缩整个包才能使用捆绑包。LZ4 使用基于块的算法，允许按段或“块”加载 AssetBundle。解压缩单个块即可使用包含的资源，即使 AssetBundle 的其他块未解压缩也不影响。

使用 `ChunkBasedCompression` 时的加载时间与未压缩捆绑包大致相当，额外的优势是减小了占用的磁盘大小。

#### BuildTarget

`BuildTarget.Standalone`：这里我们告诉构建管线，我们要将这些 AssetBundle 用于哪些目标平台。可以在关于 [BuildTarget](../ScriptReference/BuildTarget.html) 的脚本 API 参考中找到可用显式构建目标的列表。但是，如果不想在构建目标中进行硬编码，请充分利用 `EditorUserBuildSettings.activeBuildTarget`，它将自动找到当前设置的目标构建平台，并根据该目标构建 AssetBundle。

一旦正确设置构建脚本，最后便可以开始构建资源包了。如果是按照上面的脚本示例进行的操作，请单击 __Assets__ > __Build AssetBundles__ 以开始该过程。

现在已经成功构建了 AssetBundle，您可能会注意到 AssetBundles 目录包含的文件数量超出了最初的预期。确切地说，是多出了 2*(n+1) 个文件。让我们花点时间详细了解一下 `BuildPipeline.BuildAssetBundles` 产生的结果。

对于在编辑器中指定的每个 AssetBundle，可以看到一个具有 AssetBundle 名称+“.manifest”的文件。

随后会有一个额外捆绑包和清单的名称不同于先前创建的任何 AssetBundle。相反，此包以其所在的目录（构建 AssetBundle 的目录）命名。这就是清单捆绑包。我们以后会对此进行详细讨论并介绍使用方法。

#### AssetBundle 文件

这是缺少 .manifest 扩展名的文件，其中包含在运行时为了加载资源而需要加载的内容。

AssetBundle 文件是一个存档，在内部包含多个文件。此存档的结构根据它是 AssetBundle 还是场景 AssetBundle 可能会略有不同。以下是普通 AssetBundle 的结构：

![](../uploads/Main/AssetBundles-Building-0.png) 

场景 AssetBundle 与普通 AssetBundle 的不同之处在于，它针对场景及其内容的串流加载进行了优化。

#### 清单文件

对于生成的每个捆绑包（包括附加的清单捆绑包），都会生成关联的清单文件。清单文件可以使用任何文本编辑器打开，并包含诸如循环冗余校验 (CRC) 数据和捆绑包的依赖性数据之类的信息。对于普通 AssetBundle，它们的清单文件将如下所示：

```
ManifestFileVersion: 0
CRC: 2422268106
Hashes:
  AssetFileHash:
    serializedVersion: 2
    Hash: 8b6db55a2344f068cf8a9be0a662ba15
  TypeTreeHash:
    serializedVersion: 2
    Hash: 37ad974993dbaa77485dd2a0c38f347a
HashAppended: 0
ClassTypes:
- Class: 91
  Script: {instanceID: 0}
Assets:
  Asset_0: Assets/Mecanim/StateMachine.controller
Dependencies: {}
```

其中显示了包含的资源、依赖项和其他信息。

生成的清单捆绑包将有一个清单，但看起来更可能如下所示：

```
ManifestFileVersion: 0
AssetBundleManifest:
  AssetBundleInfos:
    Info_0:
      Name: scene1assetbundle
      Dependencies: {}
```

这将显示 AssetBundle 之间的关系以及它们的依赖项。就目前而言，只需要了解这个捆绑包中包含 AssetBundleManifest 对象，这对于确定在运行时加载哪些捆绑包依赖项非常有用。要了解有关如何使用此捆绑包和清单对象的更多信息，请参阅有关[本机使用 AssetBundle](AssetBundles-Native.html) 的文档。

---

<span class="page-edit">• 2017-05-15  Page published with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
