# 修补 AssetBundle

修补 AssetBundle 很简单，只需要下载新的 AssetBundle 并替换现有的 AssetBundle。如果使用 `WWW.LoadFromCacheOrDownload` 或 `UnityWebRequest` 来管理应用程序的缓存 AssetBundle，则将不同的版本参数传递给所选 API 将触发新 AssetBundle 的下载。

在修补系统中要解决的更难的问题是检测要替换的 AssetBundle。修补系统需要两个信息列表：

* 当前已下载的 AssetBundle 及其版本控制信息的列表
* 服务器上的 AssetBundle 及其版本控制信息的列表

修补程序应下载服务器端 AssetBundle 列表并比较这些 AssetBundle 列表。应重新下载缺少的 AssetBundle 或已更改版本控制信息的 AssetBundle。

也可以编写一个自定义系统来检测 AssetBundle 的更改。自己编写系统的大多数开发人员会选择对 AssetBundle 文件列表使用行业标准数据格式（例如 JSON）和并使用标准 C# 类（例如 MD5）来计算校验和。

Unity 使用以确定方式排序的数据构建 AssetBundle。因此，具有自定义下载程序的应用程序可以实现差异修补。

Unity 不提供任何内置的差异修补机制，并且 `WWW.LoadFromCacheOrDownload` 和 `UnityWebRequest` 在使用内置缓存系统时都不会执行差异修补。如果需要差异修补，则必须编写自定义下载程序。

---

* <span class="page-edit">2017-05-15  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

