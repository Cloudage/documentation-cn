常见问题解答
===================


###使用 .NET 脚本后端时不支持的类和函数

* 适用于通用 Windows 应用程序的 .NET 中不支持许多类和方法，请参阅此链接以了解支持的类：[http://msdn.microsoft.com/en-us/library/windows/apps/br230232.aspx](http://msdn.microsoft.com/en-us/library/windows/apps/br230232.aspx)。

###如何从 Visual Studio 创建 AppX 包？

* 从 Unity Editor 构建项目后，使用 Visual Studio 将其打开
* 在解决方案资源管理器中，右键单击该项目
* **Store** &gt; **Create App Packages**
* 是否要构建需要上传到 Windows 应用商店的包？选择 No，然后选择 Next
* 选择适当的平台，例如 ARM Release
* 不要包含公共符号文件，这样会使包更小
* 创建
* 找到名称类似于 YourApp_1.0.0.0_ARM_Test 的文件夹，检查并确认其具有 **Add-AppDevPackage.ps1** 文件
* 将文件夹内容复制到目标机器，然后在目标机器上右键单击 **Add-AppDevPackage.ps1** &gt; **Run with PowerShell**
* 按步骤进行操作，您可能需要互联网连接才能安装开发者许可证，这将要求您拥有 Microsoft 帐户
* 如果一切正常，应用应该会出现在开始菜单上

###如何在机器上安装 appx 文件？

* 从开始菜单中打开 **Windows PowerShell**，导航到 appx 文件，执行 **Add-AppxPackage &lt;yourappx&gt;.appx**，如果该 appx 已签名，则会在机器上安装该文件。**注意：**如果要再次安装 appx 文件，必须卸载先前安装的文件，为此只需右键单击图标，然后单击 Uninstall。

###我在部署应用程序时收到错误“DEP0600: incorrect parameter”。

* 证书有问题，请尝试通过以下操作路径创建新证书：**Package.appxmanifest** &gt; **Packaging** &gt; **Choose Certificate** &gt; **Configure Certificate** &gt; **Create Test Certificate**


###如何在 ARM 上使用 Visual Studio 图形调试器？

* 阅读此文章 [http://msdn.microsoft.com/en-us/library/hh780905(v=vs.110).aspx](http://msdn.microsoft.com/en-us/library/hh780905(v=vs.110).aspx)。需要创建一个 WinRT 组件，并从主模块引用该组件。


###如何在平板电脑上部署项目？

* 请参阅[部署](windowsstore-deployment.html)。

###如何选择用于 C# 脚本的编译器？

在通用 Windows 平台播放器设置的发布设置下，有一个名为“Compilation overrides”的下拉菜单。此处提供了 3 种设置：

    1.None。所有 C# 脚本都将使用 Mono C# 编译器进行编译；
    2.Use Net Core Partially。“Assets/Plugins”、“Assets/Standard Assets”和“Assets/Pro Standard Assets”文件夹中的脚本将使用 Mono C# 编译器进行编译，而其余的脚本将使用 Microsoft C# 编译器进行编译；
    3.Use Net Core。所有脚本都将使用 Microsoft C# 编译器进行编译。

两种编译器都有优缺点。使用 Mono C# 编译器编译脚本将允许 JavaScript 脚本引用这些脚本，例如，Angry Bots 需要这种编译方式（因此必须将其设置为 None）。但是，使用 Microsoft C# 编译器将允许使用特定于 Microsoft 的 API 而无需插件，只需将代码包装在 #if ENABLE_WINMD_SUPPORT/#endif 中，便会编译并正常工作。

###如何获取有关 Windows 应用认证工具包 (WACK) 失败的更多信息？

可在 &lt;user&gt;\AppData\Local\Microsoft\AppCertKit 中找到一个日志，其中可能包含有关失败的其他信息。

###救命啊！太多定义了！什么时候定义什么？

别担心。全部都在这里：

|:---|:---|
|__UNITY_WINRT__|Defined on all scripts|
|__UNITY_WSA__|Defined on all scripts|
|__UNITY_WINRT_10_0__|Defined on all scripts|
|__UNITY_WSA_10_0__|Defined on all scripts|
|__ENABLE_DOTNET__|Defined on all scripts when using .NET scripting backend|
|__ENABLE_IL2CPP__|Defined on all scripts when using IL2CPP scripting backend|
|__NETFX_CORE__|Defined on C# scripts that are compiled using Microsoft C# compiler when using .NET scripting backend|
|__WINDOWS_UWP__|Defined on C# scripts that are compiled using Microsoft C# compiler when using .NET scripting backend or IL2CPP scripting backend with .NET 4.6 compatibility level|
|__ENABLE_WINMD_SUPPORT__|Defined on C# scripts that are compiled using Microsoft C# compiler when using .NET scripting backend or IL2CPP scripting backend with .NET 4.6 compatibility level|

另请参阅[依赖于平台的编译](PlatformDependentCompilation.html)。

### 不能命中生成的 Assembly-CSharp-* 项目中的断点。

可能有几个原因：

* 这可能是因为模块加载时的 JIT 优化。在 Visual Studio 中，选择 __Tools__ > __Options__ > __Debugging__ > __General__，然后取消选中 __Suppress JIT optimization on module load__。
* Visual Studio 不将 Assembly-CSharp-* 视为您的代码。选择 __Tools__ > __Options__ > __Debugging__ > __General__ and uncheck __Enable Just My Code__。这样便会告诉 Visual Studio 您要调试 Assembly-CSharp-* 项目。


---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
