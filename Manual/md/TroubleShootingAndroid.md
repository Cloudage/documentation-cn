# Android 开发故障排除

在使用 Unity 进行 Android 开发时，可能会遇到许多问题。问题通常与插件或不正确的项目设置有关。本部分将概述最常见的情形和相关的故障排除建议。

## 应用程序在启动后立即崩溃

1.删除所有原生插件。

2.禁用剥离。

3.使用 `adb logcat` 从设备获取崩溃报告。参考官方 [Android 开发者 Logcat 命令行工具文档](https://developer.android.com/studio/command-line/logcat.html)以了解更多信息。

## 游戏在播放视频几秒钟后崩溃

确保设备上未启用 __Settings__ > __Developer Options__ > __Don’t keep activities__。

视频播放器有其自身的活动，因此如果激活视频播放器，常规游戏活动将遭到破坏。

## 找不到 Android 设备

如果 Unity 无法找到连接到系统的 Android 设备，请检查以下几项：

1.确保设备已实际连接到计算机 - 检查 USB 线缆和插座。

2.确保设备在 Developer options 中启用了 __USB Debugging__。有关更多详细信息，请参阅 [Android SDK/NDK 设置](android-sdksetup.html)页面。

3.从 Android SDK 安装的 `platform-tools` 目录运行 `adb devices` 命令，并检查输出。

    * 如果输出列表为空并且您使用的是 Windows，则可能需要安装 ADB 设备的驱动程序。有关更多详细信息，请参阅 [Android SDK/NDK 设置](android-sdksetup.html)文档。

    * 如果列表中包含带有 __unauthorized__ 标签的条目，可能需要在设备上授权计算机并授予其调试权限。检查设备的屏幕以查找相应的对话框。

    * 如果列表中包含带有 __device__ 标签的设备，请再次在 Unity 中构建项目。

## 无法重新打包资源

Android 资源打包工具 (AAPT) 失败时会发生此错误。AAPT 用于在 Android 构建期间构建中间资源包。此问题通常是由 Android 插件中缺少资源或资源重复引起的。

检查控制台消息以获取更多详细信息 - 其中应该会包含缺少或重复的资源的 ID。通过添加缺少的资源/设置或删除重复的插件即可修复插件中的错误。

## 无法合并 Android 清单

导致此问题的最可能原因是某个插件的清单与主 Unity 清单不兼容。

检查控制台消息以获取有关哪些属性存在冲突的更多详细信息，并相应修复清单。

请参阅 [Android 清单](android-manifest.html)文档以了解有关 Android 清单的更多详细信息。

## 无法将类转换为 DEX 格式

导致此问题的最可能原因是添加了两次 Java 插件。当 Unity 尝试从所有已编译的 Java 插件构建 DEX（Dalvik 可执行文件格式）文件时，此情况会导致重复的类。检查控制台输出以获取重复条目列表，并修复插件。

如果控制台消息显示“Too many references”，表示字段和方法的数量超过了 64k 的 DEX 限制。当插件或插件资源太多时，通常会发生这种情况。由于生成引用的方式，只需几个大插件即可达到该限制。

有几种方法可以解决此问题。其中之一是剥离插件。但是，最快的解决方法是切换到 [Gradle 构建系统](android-gradle-overview.html)，或导出项目并在 [Android Studio](https://developer.android.com/studio/index.html) 中构建项目。

## 无法将 APK 安装到设备上

此错误可能由以下原因引起：

* 安装到不兼容的设备。

* 安装设备运行的 Android 版本低于 Player settings 中的 __Minimum API Level__。

检查控制台以获取实际的错误代码和输出。

----
* <span class="page-edit">2017-05-25 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.5 版中的更新功能</span>
