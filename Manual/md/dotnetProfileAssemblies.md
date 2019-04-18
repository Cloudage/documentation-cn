# 引用其他类库程序集

如果 Unity 项目需要访问默认情况下未编译的 .NET 类库 API 的一部分，则项目可以通知 Unity 中的 C# 编译器。此行为取决于项目使用的 .NET 配置文件。

## .NET Standard 2.0 配置文件

如果项目使用 .NET Standard 2.0 __API 兼容性级别__，应该不需要采取任何其他步骤来使用 .NET 类库 API 的一部分。如果此 API 的一部分似乎丢失，可能是 .NET Standard 2.0 未随附此部分。项目可能需要改用 .NET 4.x __API 兼容性级别__。

## .NET 4.x 配置文件

默认情况下，Unity 在使用 .NET 4.x __API 兼容性级别__时引用以下程序集：

* mscorlib.dll
* System.dll
* System.Core.dll
* System.Runtime.Serialization.dll
* System.Xml.dll
* System.Xml.Linq.dll

应使用 _mcs.rsp_ 文件来引用所有其他类库程序集。可将此文件添加到 Unity 项目的 Assets 目录，然后使用该文件将其他命令行参数传递到 C# 编译器。例如，如果项目使用 `HttpClient` 类（在 _System.Net.Http.dll_ 程序集中定义），C# 编译器可能生成以下初始错误消息：

```
The type `HttpClient` is defined in an assembly that is not referenced.You must add a reference to assembly 'System.Net.Http, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'.
```

可通过将以下 mcs.rsp 文件添加到项目来解决此错误：

```
-r:System.Net.Http.dll
```

应按照以上示例中所述引用类库程序集。请勿将这些程序集复制到 Project 目录中。

## 切换配置文件

使用 _mcs.rsp_ 文件来引用类库程序集时，务必谨慎。如果将 __API 兼容性级别__从 .NET 4.x 更改为 .NET Standard 2.0，而 Project 中存在类似于以上示例的 _mcs.rsp_，则 C# 编译会失败。_System.Net.Http.dll_ 程序集并未存在于 .NET Standard 2.0 配置文件中，因此 C# 编译器无法找到该程序集。

_mcs.rsp_ 文件可能具有特定于当前 .NET 配置文件的部分。如果更改此配置文件，则需要修改 _mcs.rsp_ 文件。

---

* <span class="page-edit">2018-03-15  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>
