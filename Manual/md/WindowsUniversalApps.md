Windows 8.1 通用应用程序
==============================

|**警告：这是旧版文档** |
|:---|
|请注意，从 Unity 2017.1 开始，本文档已过时。2017-06-30|

Windows 8.1 通用应用程序是一种通过在 Windows 8.1 和 Windows Phone 8.1 上均有效的单个 Visual Studio 项目发布到 Windows 应用商店和 Windows Phone 目标平台（台式机、笔记本电脑、平板电脑和手机）的方法。这是平台融合举措的直接成果。

您可以在此处阅读更多关于 Windows 8.1 通用应用程序的信息：[http://dev.windows.com/en-us/develop/Building-universal-Windows-apps](http://dev.windows.com/en-us/develop/Building-universal-Windows-apps)

Unity 提供了一种在构建窗口中选择“Universal 8.1”SDK 来构建通用 Windows 应用商店/Phone 8.1 应用程序的方法。在构建这样的项目时，Unity 将创建一个通用 Visual Studio 项目，然后便可发布到 Windows 和 Windows Phone 设备。

工作原理
-----------------

Windows Phone 8.1 和 Windows 8.1 仍然不兼容二进制：不能在这两个平台上运行单个 DLL（除非它是可移植的类库），这意味着您无法在这两个平台上访问平台特有的 API（如 Windows Phone 上的 SMS API 和 Windows 上的鼠标 API）。因此，我们需要编译两个版本的程序集。

针对手机和应用商店编译的程序集之间有两个主要区别：预处理器指令和目标 SDK。Windows 以 Windows .NET Core 为目标，而 Phone 则以 Phone .NET Core 为目标。它们几乎完全相同，但存在一些非常细微的差异。

通用项目文件夹结构如下所示：

    UniversalApp1 - (solution directory)
        UniversalApp1.Windows - (here goes windows specific files, all Windows DLLs)
            -
            -
            -
        UniversalApp1.WindowsPhone - (here goes windows phone specific files, all Windows Phone DLLs)
            -
            -
            -
        UniversalApp1.Shared - (here goes shared files)
            -
            -
            -

构建通用应用程序时，您将从单个项目中生成两个二进制文件：一个用于 Windows，另一个用于 Windows Phone。两个 AppX 软件包都没有另一个平台留下的冗余文件，这完全归功于项目结构。


<br/>
<br/>

----------
*  <span class="page-edit">2017-06-30  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span>
