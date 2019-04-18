# 命令行参数

可以从命令行运行 Unity（从 macOS **终端**或 Windows **命令提示符**）。

在 **macOS** 上，在**终端**中输入以下命令来启动 Unity：

````
/Applications/Unity/Unity.app/Contents/MacOS/Unity
````

在 **Windows** 上，在**命令提示符**下输入以下命令来启动 Unity：

````
C:\Program Files\Unity\Editor\Unity.exe
````
以这种方式启动时，Unity 会在启动时收到命令和信息，这对于测试套件、自动构建和其他生产任务非常有用。	 
**注意**：使用相同方法来启动独立平台 Unity 游戏。

##以静默方式启动 Unity

在 **macOS** 上的**终端**中输入以下命令以静默方式启动 Unity：

````
/Applications/Unity/Unity.app/Contents/MacOS/Unity -quit -batchmode -serial SB-XXXX-XXXX-XXXX-XXXX-XXXX -username 'JoeBloggs@example.com' -password 'MyPassw0rd'
````

**注意**：使用 Jenkins 之类的连续集成 (CI) 工具通过命令行激活时，请添加 `-nographics` 标志以防止 WindowServer 错误。

在 **Windows** 上的**命令提示符**下输入以下命令以静默方式启动 Unity：

````
"C:\Program Files\Unity\Editor\Unity.exe" -quit -batchmode -serial SB-XXXX-XXXX-XXXX-XXXX-XXXX -username "JoeBloggs@example.com" -password "MyPassw0rd"
````

##将许可证退回到许可证服务器

在 **macOS** 上的**终端**中输入以下命令来退回许可证：

````
/Applications/Unity/Unity.app/Contents/MacOS/Unity -quit -batchmode -returnlicense
````

在 **Windows** 上的**命令提示符**下输入以下命令来退回许可证：

````
"C:\Program Files\Unity\Editor\Unity.exe" -quit -batchmode -returnlicense
````

##Create activation file and import license file by command

On **macOS**, type the following into the **Terminal**:

````
/Applications/Unity/Unity.app/Contents/MacOS/Unity -batchmode -createManualActivationFile -logfile

/Applications/Unity/Unity.app/Contents/MacOS/Unity -batchmode -manualLicenseFile <yourulffile> -logfile
````

On **Windows**, type the following into the **Command Prompt**:

````
"C:\Program Files\Unity\Editor\Unity.exe" -batchmode -createManualActivationFile -logfile

"C:\Program Files\Unity\Editor\Unity.exe" -batchmode -manualLicenseFile <yourulffile> -logfile
````

Check [Manual Activation Guide](ManualActivationGuide.html) for more details.
	 
##选项

可以在启动时运行 Editor 并使用其他命令和信息构建 Unity 游戏。本部分将介绍可用的命令行选项。

| **命令** | **详细信息** |
|:---|:---|
|__-assetServerUpdate &lt;IP[:port] projectName username password [r &lt;revision&gt;]&gt;__|强制更新 `IP:port` 指定的[资源服务器 (Asset Server)](AssetServer.html) 中的项目。端口是可选的，如果未指定，则假定其为标准端口 (10733)。建议将此命令与 `-projectPath` 参数结合使用，以确保使用正确的项目。如果未指定项目名称，那么命令行将使用 Unity 打开的最后一个项目。如果 `-projectPath` 指定的路径中不存在项目，则命令行会自动创建一个项目。|
|<a name="batchmode"></a>__-batchmode__|Run Unity in batch mode. You should always use this in conjunction with the other command line arguments, because it ensures no pop-up windows appear and eliminates the need for any human intervention. When an exception occurs during execution of the script code, the Asset server updates fail, or other operations fail, Unity immediately exits with return code __1__. <br/>Note that in batch mode, Unity sends a minimal version of its log output to the console. However, the [Log Files](LogFiles.html) still contain the full log information. You cannot open a project in batch mode while the Editor has the same project open; only a single instance of Unity can run at a time.<br/>__Tip__: To check whether you are running the Editor or Standalone Player in batch mode, use the [Application.isBatchMode](../ScriptReference/Application-isBatchMode.html) operator.|
|__-buildLinux32Player &lt;pathname&gt;__|构建 32 位独立平台 Linux 播放器（例如，`-buildLinux32Player path/to/your/build`）。|
|__-buildLinux64Player &lt;pathname&gt;__|构建 64 位独立平台 Linux 播放器（例如，`-buildLinux64Player path/to/your/build`）。|
|__-buildLinuxUniversalPlayer &lt;pathname&gt;__|构建 32 位和 64 位组合独立平台 Linux 播放器（例如，`-buildLinuxUniversalPlayer path/to/your/build`）。|
|__-buildOSXPlayer &lt;pathname&gt;__|构建 32 位独立平台 Mac OSX 播放器（例如，`-buildOSXPlayer path/to/your/build.app`）。|
|__-buildOSX64Player &lt;pathname&gt;__|构建 64 位独立平台 Mac OSX 播放器（例如，`-buildOSX64Player path/to/your/build.app`）。|
|__-buildOSXUniversalPlayer &lt;pathname&gt;__|构建 32 位和 64 位组合独立平台 Mac OSX 播放器（例如，`-buildOSXUniversalPlayer path/to/your/build.app`）。|
|__-buildTarget &lt;name&gt;__|允许在加载项目之前选择有效的构建目标。可能的选项包括：<br/>standalone、Win、Win64、OSXUniversal、Linux、Linux64、LinuxUniversal、iOS、Android、Web、WebStreamed、WebGL、XboxOne、PS4、PSP2、WindowsStoreApps、Switch、N3DS、tvOS 或 PSM。 |
|__-buildWindowsPlayer &lt;pathname&gt;__|构建 32 位独立平台 Windows 播放器（例如，`-buildWindowsPlayer path/to/your/build.exe`）。|
|__-buildWindows64Player &lt;pathname&gt;__|构建 64 位独立平台 Windows 播放器（例如，`-buildWindows64Player path/to/your/build.exe`）。|
|__-stackTraceLogType__|详细的调试功能。StackTraceLogging 允许进行详细日志记录。所有设置都允许选择 __None__、__Script Only__ 和 __Full__。（例如 `-stackTraceLogType Full`）|
|__-CacheServerIPAddress &lt;host:port&gt;__|在启动时连接到指定的缓存服务器，这将覆盖 Editor Preferences 中存储的配置。使用此命令可将 Unity 的多个实例连接到不同缓存服务器。|
|__-createProject &lt;pathname&gt;__|在指定路径中创建一个空项目。|
|__-editorTestsCategories__|按类别过滤 Editor 测试。使用逗号分隔测试类别。|
|__-editorTestsFilter__|按名称过滤 Editor 测试。使用逗号分隔测试名称。|
|__-editorTestsResultFile__|存放结果文件的路径位置。如果路径是文件夹，则命令行使用默认文件名。如果未指定，则将结果放在项目的根文件夹中。|
|__-executeMethod &lt;ClassName.MethodName&gt;__|Unity 打开项目后以及可选的资源服务器更新完成后，立即执行静态方法。可以使用此命令来执行连续集成，执行单元测试，进行构建或准备数据等任务。要从命令行进程返回错误，要么抛出异常（这会导致 Unity 退出并显示返回代码 __1__），要么调用 [EditorApplication.Exit](../ScriptReference/EditorApplication.Exit.html)（这会显示非零返回代码）。要传递参数，请将它们添加到命令行，并使用 `System.Environment.GetCommandLineArgs` 在函数内检索它们。要使用 `-executeMethod`，需要将包裹脚本放在 Editor 文件夹中。执行的方法必须定义为 **static**。|
|__-exportPackage &lt;exportAssetPath1 exportAssetPath2 ExportAssetPath3 exportFileName&gt;__|在指定路径（或指定路径集）的情况下导出资源包。在此示例中，`exportAssetPath` 是要从 Unity 项目导出的文件夹（相对于 Unity 项目根目录），`exportFileName` 是资源包名称。目前，此选项一次只导出整个文件夹。通常需要将此命令与 `-projectPath` 参数一起使用。|
|__-force-d3d11（仅限 Windows）__| 使 Editor 使用 Direct3D 11 进行渲染。通常，图形 API 取决于播放器设置（通常默认为 D3D11）。 |
|__-force-device-index__| 使用 Metal 时，通过传递 GPU 设备的索引，让 Editor 使用特定 GPU 设备（仅限 macOS）。|
|__-force-gfx-metal__| 使 Editor 使用 Metal 作为默认图形 API（仅限 macOS）。 |
|__-force-glcore__| 使 Editor 使用 OpenGL 3/4 Core 配置文件进行渲染。Editor 会尝试使用可用的最佳 OpenGL 版本以及 OpenGL 驱动程序公开的所有 OpenGL 扩展。如果不支持该平台，则使用 Direct3D。 |
|__-force-glcoreXY__| 与 `-force-glcore` 类似，但请求特定的 OpenGL 上下文版本。XY 的可接受值：32、33、40、41、42、43、44 或 45。 |
|__-force-gles（仅限 Windows）__| 使 Editor 使用 OpenGL for Embedded Systems 进行渲染。Editor 会尝试使用可用的最佳 OpenGL ES 版本以及 OpenGL 驱动程序公开的所有 OpenGL ES 扩展。 |
|__-force-glesXY（仅限 Windows）__| 与 `-force-gles` 类似，但请求特定的 OpenGL ES 上下文版本。XY 的可接受值：30、31 或 32。 |
|__-force-clamped__| 与 `-force-glcoreXY` 一起使用以防止检查其他 OpenGL 扩展，允许在具有相同代码路径的平台之间运行。 |
|__-force-free__| 使 Editor 运行，就像机器上有免费的 Unity 许可证一样，即使安装了 Unity Pro 许可证也是如此。|
|__-force-low-power-device__| 使用 Metal 时，让 Editor 使用低功耗设备（仅限 macOS）。|
|__-importPackage &lt;pathname&gt;__|导入指定的[资源包](HOWTO-exportpackage.html)。不显示导入对话框。|
|__-logFile &lt;pathname&gt;__|指定写入 Editor 或 Windows/Linux/OSX 独立日志文件的位置。如果省略该路径，OSX 和 Linux 将把输出写入控制台。Windows 使用路径 _%LOCALAPPDATA%\Unity\Editor\Editor.log_ 作为默认值。|
|__-nographics__|在批处理模式下运行时，完全不初始化图形设备。这样，即使在没有 GPU 的机器上也能运行自动化工作流程（自动工作流程仅在有窗口获得焦点时才起作用，否则无法发送模拟输入命令）。请注意，`-nographics` 不允许烘焙 GI，因为 Enlighten 需要 GPU 加速。|
|__-noUpm__| 禁用 Unity Package Manager。 |
|__-password &lt;password&gt;__|用户的密码（启动时需要）。|
|__-projectPath &lt;pathname&gt;__|在指定路径下打开项目。|
|__-quit__|在其他命令执行完毕后退出 Unity Editor。请注意，这可能导致错误消息被隐藏（但是，它们仍会出现在 Editor.log 文件中）。|
|__-returnlicense__|将当前激活的许可证退回到许可证服务器。在删除许可证文件前请留出几秒钟，因为 Unity 需要与许可证服务器通信。|
|__-runEditorTests__|从项目中运行 Editor 测试。这个参数需要 `projectPath`，并且最好与 `batchmode` 参数一起运行。`quit` 不是必需的，因为 Editor 在运行完后自动关闭。|
|__-serial &lt;serial&gt;__|使用指定的序列号激活 Unity。最好还能传递 `-batchmode` 和 `-quit` 参数，以便在完成时退出 Unity（如果用来自动激活 Unity）。在创建许可证文件前请留出几秒钟，因为 Unity 需要与许可证服务器通信。确保许可证文件的文件夹存在，并且在使用此参数运行 Unity 之前具有适当的权限。如果激活失败，请查看 [Editor.log](LogFiles.html) 获取信息。|
|__-setDefaultPlatformTextureFormat__|在导入纹理或构建项目之前，将默认纹理压缩设置为所需的格式。这样就不必再使用所需的格式导入纹理。可用的格式为 dxt、pvrtc、atc、etc、etc2 和 astc。<br/>请注意，这仅在 Android 上受支持。| 
|__-silent-crashes__|不显示崩溃对话框。|
|__-username &lt;username&gt;__|在激活 Unity Editor 期间，在登录窗体中输入用户名。|
|__-password &lt;password&gt;__|在激活 Unity Editor 期间，在登录窗体中输入密码。|
|__-disable-assembly-updater &lt;assembly1 assembly2&gt;__ | 指定一个以空格分隔的程序集名称列表作为 Unity 在自动更新时忽略的参数。<br/>以空格分隔的程序集名称列表是可选的：如果传递命令行选项时不带任何程序集名称，表示忽略所有程序集，如示例 1 所示。<br/><br/>示例 1<br/>`unity.exe -disable-assembly-updater`<br/><br/>示例 2 有两个程序集名称，其中一个具有路径名。示例 2 无论 `A1.dll` 存储在哪个文件夹中都会将其忽略，但只有 `A2.dll` 存储在 `subfolder` 文件夹下时才会将其忽略：<br/><br/>示例 2<br/>`unity.exe -disable-assembly-updater A1.dll subfolder/A2.dll`<br/><br/>如果在 `-disable-assembly-updater` 命令行参数中列出程序集（或者如果不指定程序集），Unity 会将以下消息记录到 [Editor.log](LogFiles.html)：<br/><br/> `[Assembly Updater] warning: Ignoring assembly [assembly_path] as requested by command line parameter.”).` <br/><br/> 可用于在导入程序集时避免不必要的 [API Updater](APIUpdater.html) 开销。<br/><br/>如果知道 Unity API 不需要更新，对于导入访问 Unity API 的程序集非常有用。在导入完全不访问 Unity API 的程序集时也很有用（例如，如果在 Unity 之外构建了游戏源代码或其中部分代码，并且想要将生成的程序集导入到 Unity 项目中）。<br/><br/>注意：如果对任何需要更新的程序集禁用更新，则可能会在编译时和/或运行时都出现难以跟踪的错误。|
|__-accept-apiupdate__|使用此命令行选项可指定在批处理模式下启动 Unity 时应运行 APIUpdater。<br/><br/>示例：<br/><br/>`unity.exe -accept-apiupdate -batchmode [other params]`<br/><br/>在批处理模式下启动 Unity 时省略此命令行参数会导致 APIUpdater 不运行，从而导致编译器错误。请注意，在 2017.2 之前的版本中，以批处理模式启动 Unity 时，无法运行 APIUpdater。|

## 示例

###C#：

````
using UnityEditor;
class MyEditorScript
{
     static void PerformBuild ()
     {
         string[] scenes = { "Assets/MyScene.unity" };
         BuildPipeline.BuildPlayer(scenes, ...);
     }
}
````

###JavaScript：

````
static void PerformBuild ()
{
    string[] scenes = { "Assets/MyScene.unity" };
    BuildPipeline.BuildPlayer(scenes, ...);
}
````

以下命令以批处理模式执行 Unity，执行 `MyEditorScript.PerformBuild` 方法，然后在完成时退出。

__Windows：__

````
C:\program files\Unity\Editor\Unity.exe -quit -batchmode -executeMethod MyEditorScript.PerformBuild
````

__Mac OS：__

````
/Applications/Unity/Unity.app/Contents/MacOS/Unity -quit -batchmode -executeMethod MyEditorScript.PerformBuild
````

以下命令以批处理模式执行 Unity，并使用提供的项目路径从资源服务器更新。从资源服务器下载并导入所有资源后执行该方法。函数执行完毕后，Unity 会自动退出。

```
/Applications/Unity/Unity.app/Contents/MacOS/Unity -batchmode -projectPath ~/UnityProjects/AutobuildProject -assetServerUpdate 192.168.1.1 MyGame AutobuildUser l33tpa33 -executeMethod MyEditorScript.PerformBuild -quit
```


##Unity Editor 特殊命令行参数

应该只在特殊情况下或者在 Unity 支持人员的指导下使用这些命令行参数。

| **命令** | **详细信息** |
|:---|:---|
|__-enableIncompatibleAssetDowngrade__|如果资源由较新的不兼容 Unity 版本制作而成，并希望将其降级以便与当前版本的 Unity 一起使用，请使用此选项。启用此选项后，如果尝试打开需要该资源的项目，Unity 会显示一个对话框，要求确认此降级。<br/>**注意：**此过程不受支持且风险很高，应仅用作最后的手段。|


##Unity 独立平台播放器命令行参数

使用 Unity 构建的独立平台播放器也能识别一些命令行参数：

| **命令** | **详细信息** |
|:---|:---|
|__-adapter N（仅限 Windows）__|允许游戏在另一个显示屏上全屏运行。N 映射到 Direct3D 显示适配器。在大多数情况下，适配器和视频卡之间存在一对一的关系。在支持多头显示器的显卡上（也就是说，可以通过一张卡驱动多个显示器），每个“头”可能是它自己的适配器。 |
|__-batchmode__|以“无头”模式运行游戏。游戏不显示任何内容，也不接受用户输入。最适合用于运行[联网游戏](UNet.html)的服务器。|
|__-force-d3d11（仅限 Windows）__| 强制游戏使用 Direct3D 11 进行渲染。 |
|__-force-d3d11-no-singlethreaded__| 强制在不使用 `D3D11_CREATE_DEVICE_SINGLETHREADED` 标志的情况下创建 DirectX 11.0。|
|__-force-device-index__| 通过向独立平台播放器传递 GPU 设备的索引，使该播放器使用特定 GPU 设备（仅限 macOS）。|
|__-force-gfx-metal__| 使独立平台播放器使用 Metal 作为默认图形 API（仅限 macOS）。 |
|__-force-glcore__| 强制 Editor 使用 OpenGL Core 配置文件进行渲染。Editor 会尝试使用可用的最佳 OpenGL 版本以及 OpenGL 驱动程序公开的所有 OpenGL 扩展。如果不支持该平台，则使用 Direct3D。|
|__-force-glcoreXY__| 与 `-force-glcore` 类似，但请求特定的 OpenGL 上下文版本。XY 的可接受值：32、33、40、41、42、43、44 或 45。 |
|__-force-clamped__| 与 `-force-glcoreXY` 一起使用，可防止检查其他 OpenGL 扩展，允许在具有相同代码路径的平台之间运行。 |
|__-force-low-power-device__| 使独立平台播放器使用低功耗设备（仅限 macOS）。|
|__-force-wayland__| 运行 Linux 播放器时激活实验性 Wayland 支持。 |
|__-nographics__|在批处理模式下运行时，完全不初始化图形设备。这样，即使在没有 GPU 的机器上也能运行自动化工作流程。|
|__-nolog（仅限 Linux 和 Windows）__|不生成输出日志。通常，`output_log.txt` 写入到游戏可执行文件旁边的 `*_Data` 文件夹中，其中会打印 [Debug.Log](../ScriptReference/Debug.Log.html) 输出。|
|__-popupwindow__|将窗口创建为弹出窗口，不带框架。|
|__-screen-fullscreen__|覆盖默认的全屏状态。此值必须是 0 或 1。|
|__-screen-height__| 覆盖默认屏幕高度。此值必须是受支持分辨率中的整数。|
|__-screen-width__|覆盖默认屏幕宽度。此值必须是受支持分辨率中的整数。|
|__-screen-quality__| 覆盖默认屏幕质量。示例用法为：`/path/to/myGame -screen-quality Beautiful`|
|__-show-screen-selector__| 强制让屏幕选择器对话框显示。|
|__-single-instance（仅限 Linux 和 Windows）__|一次只允许运行一个游戏实例。如果另一个实例已在运行，则使用 `-single-instance` 再次启动会聚焦现有实例。|
|__-parentHWND &amp;lt;HWND&gt; delayed（仅限 Windows）__|将 Windows 独立平台应用程序嵌入到另一个应用程序中。使用命令时，需要将父应用程序的窗口句柄 ('HWND') 传递给 Windows 独立平台应用程序。<br/><br/>传递 `-parentHWND 'HWND' delayed` 时，Unity 应用程序在运行时会被隐藏。还必须在应用程序中从适用于 Unity 的 [Microsoft Developer 库](https://msdn.microsoft.com/en-us/library/windows/) 中调用 [SetParent](https://msdn.microsoft.com/en-us/library/windows/desktop/ms633541(v=vs.85).aspx)。Microsoft 的 `SetParent` 会嵌入 Unity 窗口。当它创建 Unity 进程时，Unity 窗口会采用作为 Microsoft 的 [STARTUPINFO](https://msdn.microsoft.com/en-us/library/windows/desktop/ms686331(v=vs.85).aspx) 结构的一部分提供的位置和大小。<br/><br/> 要调整 Unity 窗口的大小，请在 Microsoft 的 `GetWindowLongPtr` 函数中检查其 [GWLP_USERDATA](https://msdn.microsoft.com/en-us/library/windows/desktop/ms633585(v=vs.85).aspx)。图形初始化后，其最低位设置为 1，可以安全地调整大小。显示完 Unity 启动画面后，其第二个最低位设置为 1。<br/>有关更多信息，请参阅此示例：[EmbeddedWindow.zip](../uploads/Examples/EmbeddedWindow.zip)|

##通用 Windows 平台命令行参数

通用 Windows 应用程序默认情况下不接受命令行参数，因此要传递命令行参数，必须从 **MainPage.xaml.cs/cpp** 或 ** MainPage.cs/cpp** 调用一个特殊函数。例如：

````
appCallbacks.AddCommandLineArg("-nolog");
````
	
应在 `appCallbacks.Initialize()` 函数之前调用此函数。


| **命令** | **详细信息** |
|:---|:---|
|__-nolog__| 不生成 UnityPlayer.log。|
|__-force-driver-type-warp__| 强制使用 DirectX 11.0 驱动程序类型 WARP 设备（有关更多信息，请参阅 Microsoft 的 [Windows 高级光栅化平台 (Windows Advanced Rasterization Platform)](http://msdn.microsoft.com/en-us/library/gg615082.aspx) 文档）。|
|__-force-d3d11-no-singlethreaded__| 强制在不使用 `D3D11_CREATE_DEVICE_SINGLETHREADED` 标志的情况下创建 DirectX 11.0。|
|__-force-gfx-direct__| 强制使用单线程渲染。|
|__-force-feature-level-9-3__| 强制使用 DirectX 11.0 功能级别 9.3。|
|__-force-feature-level-10-0__| 强制使用 DirectX 11.0 功能级别 10.0。|
|__-force-feature-level-10-1__| 强制使用 DirectX 11.0 功能级别 10.1。|
|__-force-feature-level-11-0__| 强制使用 DirectX 11.0 功能级别 11.0。|

---
* <span class="page-edit">2018-05-22  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span><br/>

* <span class="page-history">在 Unity [2017.2](https://docs.unity3d.com/2017.2/Documentation/Manual/30_search.html?q=newin20172) 中添加了“accept-apiupdate”命令行选项 <span class="search-words">NewIn20172</span></span>

* <span class="page-history">在 Unity [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 中添加了“-force-clamped”命令行参数 <span class="search-words">NewIn20172</span></span>

* <span class="page-history">在 [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 中停止了 Tizen 支持 <span class="search-words">NewIn20173</span></span>

* <span class="page-history">在 Unity [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 中添加了“noUpm”、“setDefaultPlatformTextureFormat”和“CacheServerIPAddress”命令行选项 <span class="search-words">NewIn20181</span></span>

* <span class="page-history">"Application.isBatchMode" operator added in 2018.2[2018.2](https://docs.unity3d.com/2018.2/Documentation/Manual/30_search.html?q=newin20181) <span class="search-words">NewIn20182</span></span>
