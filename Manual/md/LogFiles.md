#日志文件


在开发过程中，有时可能需要从构建的独立平台播放器、目标设备或 Editor 的日志中获取信息。通常，在遇到问题时需要查看这些文件，以具体了解问题的发生位置。

在 macOS 上，可通过标准 __Console.app__ 实用程序统一访问播放器和 Editor 的日志。

在 Windows 上，Editor 日志放置在默认情况下未在 Windows 资源管理器中显示的文件夹中。请参阅下文。

##Editor

要查看 Editor 日志，请在 Unity 的 Console 窗口中选择 __Open Editor Log__。

|操作系统 |日志文件 |
|:---|:---|
|**macOS** | `~/Library/Logs/Unity/Editor.log`|
|**Windows** | `C:\Users\username\AppData\Local\Unity\Editor\Editor.log`|

在 macOS 上，可通过标准 __Console.app__ 实用程序统一访问所有日志。

在 Windows 上，Editor 日志文件存储在本地应用程序数据文件夹 &lt;LOCALAPPDATA&gt;\Unity\Editor\Editor.log，其中的 &lt;LOCALAPPDATA&gt; 由 [CSIDL_LOCAL_APPDATA](https://msdn.microsoft.com/en-us/library/windows/desktop/bb762494%28v=vs.85%29.aspx) 定义。

##播放器

|操作系统 |日志文件 |
|:---|:---|
| **macOS** | `~/Library/Logs/Unity/Player.log` |
|**Windows** | `C:\Users\username\AppData\LocalLow\CompanyName\ProductName\output_log.txt`|
| **Linux** | `~/.config/unity3d/CompanyName/ProductName/Player.log` |

请注意，在 Windows 和 Linux 独立平台上，可以更改日志文件的位置（或禁止日志记录）。请参阅有关[命令行参数](CommandLineArguments.html)的文档以了解更多详细信息。

###iOS
通过 GDB 控制台或 Organizer Console 访问 XCode 中的设备日志。当应用程序未通过 XCode 调试器运行时，后一种控制台对于获取崩溃日志非常有用。

[故障排除](TroubleShootingIPhone.html)和[报告崩溃错误](iphone-bugreporting.html)指南可能对您有用。

###Android
使用 [logcat 控制台](http://developer.android.com/guide/developing/tools/adb.html#logcat)访问设备日志。为此，请使用 __Android SDK/platform-tools__ 目录中的 __adb__ 应用程序以及尾随的 __logcat__ 参数：

`$ adb logcat`

检查 LogCat 的另一种方法是使用 [Dalvik Debug Monitor Server (DDMS)](http://developer.android.com/guide/developing/debugging/ddms.html)。可从 __Eclipse__ 启动或从 __Android SDK/tools__ 内部启动 DDMS。DDMS 还提供了许多其他与调试相关的工具。

### 通用 Windows 平台

|设备 |日志文件 |
|:---|:---|
|**桌面端** |`%USERPROFILE%\AppData\Local\Packages<productname>\TempState\UnityPlayer.log`|
|**Windows Phone**| 可使用 [Windows Phone Power Tools](https://wptools.codeplex.com/) 进行检索。也可使用 [Windows Phone IsoStoreSpy](https://isostorespy.codeplex.com/)。 |

###WebGL

在 WebGL 上，日志输出将写入浏览器的 [JavaScript 控制台](webgl-debugging.html)。

##在 Windows 上访问日志文件

在 Windows 上，日志文件存储在默认情况下为隐藏状态的位置。在 Windows XP 的 Windows 资源管理器中，使用 __Tools__ > __Folder Options...__ > __View（选项卡）__ 即可显示隐藏的文件夹。

在 Windows Vista/7 的 Windows 资源管理器中，请使用 __Tools__ > __Folder Options...__ > __View（选项卡）__ 来显示 AppData 文件夹。默认情况下会隐藏 Tools 菜单；按一次 Alt 键即可显示。

---
* <span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>

* <span class="page-history">在 [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 中停止了 Tizen 支持 <span class="search-words">NewIn20173</span></span>

