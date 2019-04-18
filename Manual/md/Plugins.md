#插件

在 Unity 中，通常使用脚本来创建功能，但也可使用__插件__形式包含 Unity 外部创建的代码。可在 Unity 中使用两种插件：_托管插件_和_原生插件_。



_托管插件_是使用 Visual Studio 等工具创建的托管 .NET 程序集。此类插件仅包含 .NET 代码，因此无法访问 .NET 库不支持的任何功能。但是，Unity 使用的标准 .NET 工具可以访问托管代码来编译脚本。因此，托管插件代码和 Unity 脚本代码之间几乎没有用法差异，唯一的区别是插件在 Unity 外部编译而成，因此源代码可能不可用。

_原生插件_是特定于平台的本机代码库。此类插件可访问操作系统调用和第三方代码库等功能；Unity 无法通过其他方式使用这些功能。但是，Unity 的工具无法以托管库的方式访问这些库。例如，如果忘记将托管插件文件添加到项目中，您将收到标准编译器错误消息。如果也忘记将原生插件添加到项目中，则只会在尝试运行项目时看到错误报告。

本部分将介绍如何创建插件以及在 Unity 项目中使用插件。

---
* <span class="page-edit">2018-03-19  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">从 2018.1 开始，MonoDevelop 由 Visual Studio 取代</span>
