# 对 APK 扩展文件 (OBB) 的支持

APK 扩展文件用作应对 Google Play 应用商店 100MB 应用大小限制的解决方案。如果应用程序大于 100MB（很可能是大型游戏），必须将输出包拆分为主要部分 (APK) 和扩展文件 (OBB)。请参阅 Android 开发者文档的[扩展文件](https://developer.android.com/google/play/expansion-files.html)部分以了解更多信息。

Unity 会自动将输出包拆分为 APK 和 OBB。这不是拆分应用程序包的唯一方法（其他选项包括第三方插件和 [AssetBundle](https://docs.unity3d.com/Manual/AssetBundlesIntro.html)），但却是 Unity 官方支持的唯一自动拆分机制。

## 构建带扩展文件的应用程序

如果希望 Unity 将应用程序输出包拆分为 APK 和 OBB，请打开 __Player Settings__ 窗口（菜单：__Edit__ > __Project Settings__ > __Player__），然后在 __Publishing Settings__ 部分勾选 __Split Application Binary__ 复选框。

![__Player Settings__ 窗口的 __Publishing Settings__ 部分，其中突出显示了 __Split Application Binary__ 复选框](../uploads/Main/android-OBB-0.png)

输出包的两个部分（APK 和 OBB）都将复制到构建应用程序时指定的输出目录。例如，如果 APK 的名称为 _mygame.apk_，则 OBB 位于同一目录中且命名为 _mygame.main.obb_。

如果选择 __Build and Run__，则 Unity 会在设备上安装 APK 和 OBB 文件。如果选择 __Build__ 并希望使用 ADB 实用程序手动安装应用程序，必须先安装 APK，然后将 OBB 复制到设备上的正确位置。OBB 文件名必须符合 Google 要求的格式。请参阅 Android 开发者文档的[扩展文件](https://developer.android.com/google/play/expansion-files.html)部分以了解更多信息。

如果应用程序启动时无法找到和加载 OBB，则只有第一个场景可用（有关更多信息，请参阅以下有关如何在 APK 和 OBB 之间拆分数据的文档）。不要单独使用 OBB 的内容，始终应将 APK 和 OBB 视为唯一的捆绑包，与处理单个 APK 的方式相同。

## 如何在 APK 和 OBB 之间拆分数据

启用 __Split Application Binary__ 选项后，将按以下方式拆分应用程序：

* __APK__ - 由可执行文件（Java 和本机文件）、插件、脚本以及第一个场景（索引为 0）的数据组成。

* __OBB__ - 包含其他所有内容，包括所有剩余的场景、资源和流媒体资源。

如果 APK 仍然太大而无法在 Google Play 应用商店中发布（超过 100MB），请尝试缩小第一个场景的大小，使其尽可能小。

## 下载 OBB 扩展文件

[Unity Asset Store 提供了一个插件](https://www.assetstore.unity3d.com/en/#!/content/3189)，让您访问适用于 Unity 的 Google Play `market_downloader` 库的改编版本，使用该库可从 Google Play 应用商店或外部来源下载 OBB，然后将其移动到正确的目录。

## 在 Google Play 应用商店上托管 OBB 文件

OBB 扩展文件应与 APK 一起发布到 Google Play 应用商店。当用户从 Google Play 应用商店中安装应用程序时，将自动下载随 APK 一起发布的所有 OBB 文件。

您应该在应用程序中提供代码用于在 Google Play 应用商店出错的情况下或用户从其设备中删除 OBB 文件的情况下能够下载缺失的 OBB 文件。有关下载 OBB 文件的更多信息，请参阅 Android 开发者文档的 [APK 扩展文件 (APK Expansion file)](https://developer.android.com/google/play/expansion-files.html#DownloadProcess) 部分。

## 不使用 Google Play 应用商店托管 OBB 文件

如果您不想使用 Google Play 应用商店，也可以自己托管 OBB 文件。但是，仅建议高级用户不使用 Google Play 应用商店来托管 OBB 文件。

----
* <span class="page-edit">2017-05-25 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.5 版中的更新功能</span>
