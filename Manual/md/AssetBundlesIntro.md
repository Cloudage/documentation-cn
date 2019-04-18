# AssetBundle

__AssetBundle__ 是一个存档文件，包含可在运行时加载的特定于平台的资源（模型、纹理、预制件、音频剪辑甚至整个场景）。AssetBundle 可以表达彼此之间的依赖关系；例如 AssetBundle A 中的材质可以引用 AssetBundle B 中的纹理。为了通过网络进行有效传递，可以根据用例要求选用内置算法来压缩 AssetBundle（LZMA 和 LZ4）。

AssetBundle 可用于可下载内容（DLC），减小初始安装大小，加载针对最终用户平台优化的资源，以及减轻运行时内存压力。

## AssetBundle 中有什么？

问得好，实际上“AssetBundle”可以指两种不同但相关的东西。

首先是磁盘上的实际文件。对于这种情况，我们称之为 AssetBundle 存档，在本文档中简称“存档”。存档可以被视为一个容器，就像文件夹一样，可以在其中包含其他文件。这些附加文件包含两种类型：序列化文件和资源文件。序列化文件包含分解为各个对象并写入此单个文件的资源。资源文件只是为某些资源（纹理和音频）单独存储的二进制数据块，允许我们有效地在另一个线程上从磁盘加载它们。

其次是通过代码进行交互以便从特定存档加载资源的实际 AssetBundle 对象。此对象包含一个映射，即从已添加到此存档的资源的所有文件路径到按需加载的资源所包含的对象之间的映射。

<br/> 

----

* <span class="page-edit">2017-05-15  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>
