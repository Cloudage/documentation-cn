Visual Studio C# 集成
============================

使用 Visual Studio 的好处
------------------------------

一种更完善的 C# 开发环境。
智能自动补齐、基于计算机辅助的源文件更改、智能语法突出显示等。

Community、Express 和 Pro 版本之间的差异
----------------------------------------------

VisualStudio C# 是 Microsoft 推出的集成开发环境 (IDE) 工具。
Visual Studio 现在有三个版本：[Community](https://www.visualstudio.com/vs/community/)（免费使用）、[Professional](http://www.microsoft.com/visualstudio/en-us/products/professional/default.mspx)（付费）和 [Enterprise](https://www.visualstudio.com/vs/enterprise/)（付费）。[Visual Studio 网站](https://www.visualstudio.com/vs/compare/)上提供了版本之间功能差异的比较。

通过在 Unity 中集成 Visual Studio，可以自动创建和维护 Visual Studio 项目文件。此外，在双击脚本或 Unity 控制台中的错误消息时，VisualStudio 将会打开。

Visual Studio 与 Unity 结合使用
------------------------------------------------
请按照以下步骤配置 Unity Editor 以便将 Visual Studio 用作其默认 IDE：

在 Unity 中，选择 __Edit > Preferences__，并确保选择 Visual Studio 作为首选的外部编辑器。


![External Tool 设置](../uploads/Main/external_tools.png)

接下来，双击项目中的 C# 文件。Visual Studio 应该会自动打开该文件。

可以编辑文件、保存文件并切换回 Unity 来测试更改。

有几点需要注意
------------------------------

* 尽管 Visual Studio 附带了自己的 C# 编译器，并且您可以使用它来检查 # 脚本中是否存在错误，但 Unity 仍然使用自己的 C# 编译器来编译脚本。使用 Visual Studio 编译器仍然非常有用，因为这意味着不必一直切换到 Unity 来检查是否有任何错误。


* Visual Studio 的 C# 编译器比 Unity 的 C# 编译器目前支持的功能更多。也就是说，某些代码（尤其是较新的 # 功能）不会在 Visual Studio 中抛出错误，但在 Unity 中则会。


* Unity 会自动创建和维护 Visual Studio .sln 和 .csproj 文件。每当在 Unity 中添加/重命名/移动/删除文件时，Unity 都会重新生成 .sln 和 .csproj 文件。也可以从 Visual Studio 向解决方案添加文件。Unity 随后会导入这些新文件，下次 Unity 再次创建项目文件时，便会使用包含的新文件进行创建。
