# 导出前和导出后方法

导出前和导出后方法允许您在 Unity 项目编译之前和之后触发操作。这些方法必须作为代码存在于项目内的 Asset 文件夹的 Editor 文件夹中。如果目录中不存在此文件夹，请在 Asset 文件夹中右键单击，然后转至 __Create__>__Folder__ 并将其命名为 "Editor"。

__重要信息__：UnityEngine.CloudBuild.BuildManifestObject 类仅在 Cloud Build 中（在其中而在非本地）运行时才可用。要在本地编译您的代码，请将导出前和导出后方法放入 `#if UNITY_CLOUD_BUILD` 块中。

应在编译目标的 Advanced Options（高级选项）中设置导出前和导出后方法。有关更多信息，请参阅[高级选项](UnityCloudBuildAdvancedOptions.html)。

![Edit Advanced Options 屏幕](../uploads/Main/UnityCloudBuildAdvancedOptions-AdvancedOptionsEdit.png)

__导出前方法名称__

要使用导出前方法，请在 Unity 项目中创建一个公有静态方法，并在其中添加需要在 Unity Editor 导出项目之前执行的代码。

`public static void PreExport()`

通过将 BuildManifestObject 对象指定为方法签名中的参数，即可使 Cloud Build 将当前编译的编译清单传递到导出前方法。随后可以在导出项目之前更改项目或玩家设置。

`public static void PreExport(UnityEngine.CloudBuild.BuildManifestObject manifest)`

Unity Cloud Build 调用该方法时，它会将 BuildManifestObject 对象作为可选参数传递（其中的 BuildManifestObject 是当前编译的编译清单）。

有关更多信息，请参阅 [ScriptableObject 格式的编译清单](UnityCloudBuildManifestAsScriptableObject.html)。

## __导出后方法名称__

要使用导出后方法，请在 Unity 项目中创建一个公有静态方法，并在其中添加需要在 Unity Editor 导出项目之后执行的代码。

`public static void PostExport(string exportPath)`

Unity Cloud Build 调用该方法时，它将传递字符串：

* 对于非 iOS 编译目标，该字符串包含导出的项目的路径。

* 对于 iOS 项目，该字符串包含导出的 Xcode 项目的路径。在调用 Xcode 以完成编译过程之前，可使用此路径找到导出的 Xcode 项目，从而执行额外的预处理。

__注意__：如果已使用 Unity [PostProcessBuildAttribute](../ScriptReference/Callbacks.PostProcessBuildAttribute.html) 标记了代码中的任何方法，那么这些方法的执行时间将先于 Unity Cloud Build 中任何配置为导出后方法的方法。

