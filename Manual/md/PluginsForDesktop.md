构建适用于桌面平台的插件
======================================


本页面将介绍适用于桌面平台 (Windows/Mac OS X/Linux) 的[原生代码插件](Plugins.html)。


构建适用于 Mac OS X 的插件
------------------------------


在 Mac OSX 上，以捆绑包的形式部署[插件](Plugins.html)。可使用 XCode 按以下方法创建捆绑包项目：选择 __File &gt; NewProject...__，然后选择 __Bundle__ &gt; __Carbon/Cocoa Loadable Bundle__（使用 XCode 3）或 __OS X__ &gt; __Framework & Library__ &gt; __Bundle__（使用 XCode 4）

如果使用 C++ (.cpp) 或 Objective-C (.mm) 来实现该插件，则必须确保使用 C 链接来声明函数以免发生[名称错用问题](http://en.wikipedia.org/wiki/Name_mangling)。



````
extern "C" {
  float FooPluginFunction ();
}

````

构建适用于 Windows 的插件
-----------------------------


Windows 上的插件是具有导出函数的 DLL 文件。实际上，任何可创建 DLL 文件的语言或开发环境均可用于创建插件。
与 Mac OSX 一样，应使用 C 链接来声明所有 C++ 函数以免发生名称错用问题。

构建适用于 Linux 的插件
---------------------------


Linux 上的插件是具有导出函数的 `.so` 文件。这些库通常用 C 或 C++ 编写而成，但也可使用任何语言编写。
与其他平台一样，应使用 C 链接来声明所有 C++ 函数以免发生名称错用问题。

32 位和 64 位库
---------------------------

对于需要 32 位和/或 64 位插件的问题，处理方式因平台而异。

###Windows 和 Linux
在 Windows 和 Linux 上，可以手动管理插件（例如，在构建 64 位播放器之前，将 64 位库复制到 `Assets/Plugins` 文件夹中，在构建 32 位播放器之前，将 32 位库复制到 `Assets/Plugins` 文件夹中），或者可以将 32 位版本的插件放在 `Assets/Plugins/x86` 中，将 64 位版本的插件放在 `Assets/Plugins/x86_64` 中。默认情况下，Editor 将首先查看特定于架构的子目录，如果该目录不存在，则从 `Assets/Plugins` 根文件夹中复制插件。

请注意，对于通用 Linux 构建，需要使用特定于架构的子目录（在进行通用 Linux 构建时，Editor 不会从 `Assets/Plugins` 根文件夹中复制任何插件）。

###Mac OS X
对于 Mac OS X，应将插件构建为包含 32 位和 64 位架构的通用二进制文件。

使用 C# 中的插件
-------------------------


构建捆绑包后，应将其放在 Unity 项目的 __Assets &gt; Plugins__ 文件夹中（或适当的特定于架构的子目录中）。在 C# 脚本中定义如下所示的函数时，Unity 将按名称查找捆绑包：



````
[DllImport ("PluginName")]
private static extern float FooPluginFunction ();

````

请注意，__PluginName__ 不应包含库前缀和文件扩展名。例如，插件文件的实际名称在 Windows 上为 PluginName.dll，在 Linux 上为 libPluginName.so。
请注意，每次更改插件中的代码时，都需要在项目中重新编译脚本，否则插件将没有最新的编译代码。

部署
----------


对于跨平台插件，必须在 Plugins 文件夹中包含 .bundle（对于 Mac）、.dll（对于 Windows）和 .so（对于 Linux）文件。
然后便不需要您执行任何其他操作，Unity 会自动为目标平台选择正确的插件并将其包含在播放器中。


示例
--------


###最简单的插件
此插件项目只实现了一些非常基本的操作（打印一个数字，打印一个字符串，将两个浮点数相加，将两个整数相加）。如果这是您的第一个 Unity 插件，此示例可能会有所帮助。
此项目位于[此处](../uploads/Examples/SimplestPluginExample-4.0.zip)，其中包含 Windows、Mac 和 Linux 项目文件。

###使用 C++ 代码渲染
[原生插件接口](NativePluginInterface.html)页面上提供了一个在 Unity 中使用多线程渲染技术的多平台插件示例。

