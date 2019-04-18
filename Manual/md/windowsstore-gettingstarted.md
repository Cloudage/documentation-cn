入门
==============================


Unity 在面向通用 Windows 平台时支持三种 CPU 架构：X86、X64 和 ARM。

有三种配置（在 Visual Studio 中选择它们）：

* **Debug**，用于调试
* **Release**，用于性能分析
* **Master**，用于提交到应用商店

播放器日志位于 &lt;user&gt;\AppData\Local\Packages\&lt;productname&gt;\TempState 下。另请参阅[日志文件](LogFiles.html)。

面向通用 Windows 平台时的要求：

* Windows 8.1
* Visual Studio 2015（RTM 或更高版本）
* Windows 10 Universal SDK
* 测试设备：
    * 对于 Windows SDK 10 的基本测试，使用开发所用的同一台 PC 就足够了。
    * 对于 Phone 10 的基本测试，建议使用真正的 Windows Phone 10 设备。

在继续操作之前，需要获取 Windows 应用商店开发者许可证。有两种方式可以做到这一点：

* 从 Visual Studio 构建一个空的通用 Windows 应用程序并进行部署，如果是第一次执行此操作，应该会打开用于获取开发者许可证的对话窗口，然后按照步骤操作。
* 请参阅此链接 - [http://msdn.microsoft.com/en-us/library/windows/apps/hh974578.aspx#getting_a_developer_license_by_using_visual_studio](http://msdn.microsoft.com/en-us/library/windows/apps/hh974578.aspx#getting_a_developer_license_by_using_visual_studio)

不支持以下方面：

* 旧版 Network 类（请使用当前的 [Unity 网络](UNet.html)）。支持 WWW 和 UnityWebRequest（适用于所有脚本后端）。
* `GameObject.SendMessage` 不完全有效，但接受消息的函数必须与发送的消息匹配，因为参数转换不起作用（仅适用于 .NET 脚本后端）。
* 无法从 JS 脚本自动访问 C# 类（仅适用于 .NET 脚本后端）。要执行此操作，请访问 PlayerSettings（菜单：__File__ > __Project Settings__ > __Player__），打开 __Publishing Settings__，导航到 __Compilation__ 部分，并将 __Compilation Overrides__ 设置为 __.NET Core Partially__。


###实用链接

* 通用 Windows 应用程序一般性示例 [https://github.com/Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples)
* 通用 Windows 应用程序的一般性论坛 [https://social.msdn.microsoft.com/Forums/windowsapps/en-us/home?forum=wpdevelop](https://social.msdn.microsoft.com/Forums/windowsapps/en-us/home?forum=wpdevelop)
* 专门探讨 Windows 开发的 Unity 论坛 [http://forum.unity3d.com/forums/50-Windows-Development](http://forum.unity3d.com/forums/50-Windows-Development)

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
