# Xcode framework

如果您的 iOS 项目需要额外的 Xcode framework，请使用 [PBXProject](../ScriptReference/iOS.Xcode.PBXProject.html) API 将这些框架添加到由 Unity Cloud Build 创建的 Xcode 项目文件。

可通过两种方法调用此 API 来操作 Xcode 项目：

* 使用内置的 Unity [PostProcessBuildAttribute](../ScriptReference/Callbacks.PostProcessBuildAttribute.html)（在 Unity Cloud Build 导出后方法运行之前执行它）。

* 使用 [Unity Cloud Build 导出后方法](UnityCloudBuildPreAndPostExportMethods.html)。

**注意**：如果您使用的是早于 5.0 的 Unity 版本，请使用 Unity 在 [Bitbucket](https://bitbucket.org/Unity-Technologies/xcodeapi) 上提供的 [Xcode Manipulation API](https://bitbucket.org/Unity-Technologies/xcodeapi) 将此功能添加到 Unity 项目。
