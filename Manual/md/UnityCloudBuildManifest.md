# 编译清单

让游戏的运行时代码了解关于编译本身的关键信息通常很有用。报告错误或跟踪分析数据时，编译的名称和编号等信息非常有用。为了帮助实现这一点，Unity Cloud Build 在编译时将 "清单" (manifest) 注入到游戏中，以便稍后在运行时能够访问这些关键数据。

Unity Cloud Build 清单以 JSON 格式的 [TextAsset](class-TextAsset.html) 形式提供。此清单存储为游戏资源，可通过 `Resources.Load()` 访问。编译清单包含以下值：

|**值：** |**属性：** |
|:---|:---|
|`scmCommitId`|	已编译的提交或变更列表。|
|`scmBranch`|	已编译的分支的名称。|
|`buildNumber`|	与此编译相对应的 Unity Cloud Build "编译号" (build number)。|
|`buildStartTime`|	编译过程开始时的 UTC 时间戳。|
|`projectId`|	Unity 项目标识符。|
|`bundleId`|	Unity Cloud Build 中配置的 `bundleIdentifier`（仅限 iOS 和 Android）。|
|`unityVersion`|	Unity Cloud Build 用于创建该编译的 Unity 版本。|
|`xcodeVersion`|	用于编译项目的 XCode 版本（仅限 iOS）。|
|`cloudBuildTargetName`|	已编译的编译目标的名称。|

称为 __UnityCloudBuildManifest.json__ 的清单 TextAsset 会写入到 _Assets/UnityCloud/Resources_ 文件夹中。

##对于本地测试

要在本地测试编译清单功能，请将文件命名为 **UnityCloudBuildManifest.json.txt**（但是不要将此文件提交到项目代码仓库中的 **Assets/UnityCloud/Resources** 文件夹，因为它可能会干扰 Unity Cloud Build 清单文件）。

##使用清单

可通过以下途径在运行时访问清单：

* [JSON 格式的清单](UnityCloudBuildManifestAsJSON.html)

* [ScriptableObject 格式的清单](UnityCloudBuildManifestAsScriptableObject.html)
