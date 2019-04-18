#通用 Windows 平台：.NET 脚本后端上的插件

## 通用 Windows 平台插件设置

要查看这些设置，请转到 Unity Editor 的 Project 窗口，选择插件文件，然后在 Inspector 窗口中导航到 __Platform settings__ &gt; __Universal Windows Platform__（Windows 图标）。

![通用 Windows 平台插件设置](../uploads/Main/PluginInspectorWSATab.png)

|**_属性：_** |**_功能：_** |
|:---|:---|
|__SDK__ |使用下拉选单使插件与 __Any SDK__ 或特定 SDK 兼容。 |
|__CPU__ |使用下拉选单使插件与 __Any CPU__ 兼容，或将插件限制为 __32-bit__、__64-bit__ 或 __ARM__ 播放器。 |
|__Don't process__ <br/>（仅适用于托管程序集） |勾选此复选框可禁用此程序集的修补。当程序集包含 Unity 可序列化的类时，程序集需要修补。在这些情况下，Unity 会在程序集内注入额外的 IL 代码。如果您知道程序集没有这些类，那么禁用修补是安全的。<br/>**注意：**Unity 会将序列化代码注入到程序集内，因此如果插件中有一个派生自 MonoBehaviour 的类，而 Unity 不对其进行修补，则可能会在运行时出现序列化错误。 |
|__Placeholder__ <br/>（仅适用于托管程序集） | 使用通用 Windows 平台的情况下，可根据 .NET Core 编译插件，但由于 Unity Editor 在 Mono 上运行，因此无法识别这些程序集。导致的结果是 C# 或 JS 文件无法引用它们。为解决此问题，需提供根据 .NET 3.5 进行编译并具有相同 API 的程序集，它将充当真实插件的占位插件（请参阅下一部分：_占位插件_）。 |

请参阅有关 [Plugin Inspector](PluginInspector.html) 的文档以了解更多信息。

## 占位插件

如果使用 [Windows 运行时 API](http://msdn.microsoft.com/en-us/library/windows/apps/br211377.aspx)，则无法在 Unity Editor 中使用特定于通用 Windows 平台的插件。本部分将介绍如何在 Unity Editor 中处理此问题。

如果只打算对通用 Windows 平台使用插件而不在 Unity Editor 中使用插件，则不需要创建占位插件，但需要使用以下语句对使用插件 API 的代码进行包装：

```
# if !UNITY_EDITOR
// 插件代码
# endif
```

如果打算将插件同时用于通用 Windows 平台和 Unity Editor，则需要占位插件。创建两个插件：

* 对于**通用 Windows 平台**，创建一个根据 .NET Core 进行编译的程序集插件，它具有内置的 Windows 运行时 API。
* 对于 **Unity Editor**，创建一个根据 .NET 3.5 进行编译的程序集插件，它具有相同的公共 API 并采用虚函数实现（这是占位插件）。

这两个插件必须共享相同的名称并具有相同的程序集版本。请注意，Unity Editor 的占位插件不能引用 _UnityEditor.dll_。如果进行此引用，Unity 会产生错误。

以下步骤描述了如何在 Editor 中为每个插件分配平台。

1.在 Unity Editor 的 Project 窗口中，选择与 Editor 兼容的占位插件。在 Inspector 窗口中，选择 __Select platforms for plugin__，并选择 __Editor__ 作为唯一兼容的平台。

1.在 Unity Editor 的 Project 窗口中，选择与通用 Windows 平台兼容的占位插件。在 Inspector 窗口中，选择 __Select platforms for plugin__，并选择 __Universal Windows Platform__ 作为唯一兼容的平台。

1.在与通用 Windows 平台兼容的插件的 Inspector 窗口中，将 __Placeholder__ 字段设置为与 Editor 兼容的占位插件。

这意味着在进行以通用 Windows 平台为目标的构建时，Unity 在编译脚本时使用与 Editor 兼容的占位插件，而将与通用 Windows 平台兼容的插件复制到最终文件夹。这种机制实现了两个目的：Unity Editor 成功编译脚本，但构建的游戏本身仍然使用通用 Windows 平台特有插件中的 API。

---
<span class="page-edit">• 2017-05-16  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span><br/>
