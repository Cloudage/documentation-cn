#通用 Windows 平台播放器设置 (Universal Windows Platform Player Settings)

本页面将详细介绍通用 Windows 平台特有的__播放器设置 (Player Settings)__。有关常规播放器设置的描述，请参阅[播放器设置](class-PlayerSettings.html)相关文档。

![](../uploads/Main/WSA-PlayerSettings.png) 


首次创建 Visual Studio 解决方案时，大多数这些设置都会传输到 _Package.appxmanifest_ 文件。

注意：如果在现有项目之上构建项目，Unity 将不会覆盖 _Package.appxmanifest_ 文件（如果已存在）。这意味着，如果在 Player Settings 中更改了某些内容，则应确保选中 _Package.appxmanifest_。如果要重新生成 _Package.appxmanifest_，请将其删除并从 Unity 重新构建项目。

要了解更多信息，请参阅 Microsoft 的[应用程序包清单](http://msdn.microsoft.com/en-us/library/windows/apps/br211474.aspx)文档。

来自 Packaging、Application UI、Tile、Splash screen 和 Capabilities 的设置将直接传输到 _Package.appxmanifest_ 文件中的设置。

Player Settings 中**支持的方向 (Supported orientations)** 也会填充到清单（Visual Studio 解决方案中的 Package.appxmanifest 文件）。在通用 Windows 应用程序上，Unity 会将方向重置为您在 Player Settings 中使用的方向，无论在清单中指定了什么设置均是如此。这是因为 Windows 本身将忽略台式机和平板电脑上的这些设置。请注意，始终可使用 Unity Scripting API 来更改支持的方向。

##Certificate

每个通用 Windows 应用程序都需要一个标识开发者的证书，如果您未提供自己的证书，Unity 将创建默认证书。

##Compilation

如您所知，Unity 在编译脚本文件时使用 Mono，而您可以使用 .NET 3.5 中的 API。Compilation Overrides 允许在 C# 文件中使用适用于通用 Windows 平台的 .NET（也称为 .NET Core），此处提供了 API。

##当 Compilation Overrides 设置为：

* None - 使用 Mono 编译器编译 C# 文件。
* Use .Net Core - 使用 Microsoft 编译器和 .NET Core 编译 C# 文件，您可以使用 Windows 运行时 API，但无法通过 JS 语言访问在 C# 文件中实现的类。注意：使用来自 Windows 运行时的 API 时，建议使用 ENABLE_WINMD_SUPPORT 定义来包装代码，因为该 API 仅在构建到通用 Windows 平台时可用，在 Unity Editor 中不可用。
* Use .Net Core Partially - 使用 Microsoft 编译器和 .NET Core 编译不在 Plugins、Standard Assets、Pro Standard Assets 文件夹中的 C# 文件，而使用 Mono 编译器编译所有其他 C# 文件。优点是可通过 JS 语言访问 C# 中实现的类。
注意：您将无法在 Unity Editor 中测试 .NET Core API，因为它无法访问 .NET Core，所以您只能在运行通用 Windows 应用程序时测试该 API。

**注意：**不能在 JS 脚本中使用 .NET Core API。

以下是一个如何在脚本中使用 .NET Core API 的简单示例。

````
string GetTemporaryFolder()
{
# if ENABLE_WINMD_SUPPORT
    return Windows.Storage.ApplicationData.Current.TemporaryFolder.Path;
# else
    return "LocalFolder";
# endif
}
````

##Misc

Unprocessed Plugins 包含 Unity 预处理工具（如 SerializationWeaver、AssemblyPreprocessor、rrw）忽略的插件列表。通常情况下，您不需要修改此列表，除非收到 Unity 未能预处理插件的错误。

如果在此列表中添加一个插件，Unity 不会将额外的 IL 代码注入用于序列化的程序集，但是如果插件没有引用 UnityEngine.dll，那没问题，因为 Unity 不会序列化插件的任何数据。

##Independent Input Source
此属性允许启用独立输入源的选项。基本上这会提高输入响应能力，通常都希望启用此选项。

Low Latency Presentation API
允许启用 Low Latency Presentation API，基本上此功能可创建带 DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT 标志的 D3D11 交换链，在此处读取更多内容，并应该能提高输入响应能力。默认情况下会禁用此选项，因为在具有较旧 GPU 驱动程序的硬件上，如果启用此选项，此选项会使游戏发生延迟；务必分析游戏的性能是否仍在可接受范围。

##Capabilities
这些选项直接复制到 Package.appxmanifest。

**注意：**如果在先前的包之上构建游戏，不会覆盖 Package.appxmanifest。

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
