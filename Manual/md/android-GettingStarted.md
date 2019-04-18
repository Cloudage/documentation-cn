# Android 开发入门

Unity 手册的 [Android 环境设置](https://docs.unity3d.com/Manual/android-sdksetup.html)主题包含了在 Android 设备或 Android 模拟器上运行代码之前必须完成的任务的基本概述。有关设置 Android 开发环境的更多深入信息，请参阅 [Android 开发者门户](http://developer.android.com/sdk)上的分步说明。

如果在安装过程中遗漏了某些必需项目，Unity 会在进行 Android 项目构建时验证您的开发环境，并提示您升级或下载缺少的组件。

Unity 提供了脚本 API 来访问 Android 设备的各种输入数据和设置。

请参阅本手册的 [Android 脚本页面](https://docs.unity3d.com/Manual/android-API.html)以了解更多信息。

## 将原生 C、C++ 或 Java 代码暴露给脚本

可使用插件直接从 C# 脚本调用使用 C/C++ 编写的 Android 函数（可间接调用 Java 函数）。

要了解如何使这些函数在 Unity 中可访问，请访问 [Android 插件页面](https://docs.unity3d.com/Manual/PluginsForAndroid.html)。

## 遮挡剔除

Unity 支持遮挡剔除，这是一种对移动平台非常有价值的优化方法。

请参阅本手册的 [遮挡剔除](https://docs.unity3d.com/Manual/OcclusionCulling.html)页面以了解更多信息。

## 自定义启动画面

您可以自定义在 Android 上启动游戏时显示的启动画面。

请参阅本手册的[自定义 Android 启动画面](https://docs.unity3d.com/Manual/AndroidMobileCustomizeSplashScreen.html)页面以了解更多信息。

## 故障排除和错误报告

[Android 故障排除指南](https://docs.unity3d.com/Manual/TroubleShootingAndroid.html)可帮助您尽快发现错误原因。如果在查阅指南后怀疑问题是由 Unity 引起的，请根据 Unity 错误报告指南提交错误报告。

请参阅 [Android 错误报告页面](https://docs.unity3d.com/Manual/android-bugreporting.html)了解有关提交错误报告的详细信息。

<a name="compression"></a>
## 纹理压缩

Android 上的标准纹理压缩格式为 Ericsson 纹理压缩 (ETC)。

所有当前的 Android 设备都支持 ETC1 版本，但该版本不支持具有 Alpha 通道的纹理。所有支持 OpenGL ES 3.0 的 Android 设备都支持 ETC2 版本。该版本可提供改进的 RGB 纹理质量，并且还支持具有 Alpha 通道的纹理。

默认情况下，Unity 对压缩的 RGB 纹理使用 ETC1，而对压缩的 RGBA 纹理使用 ETC2。如果 Android 设备不支持 ETC2，则纹理将在运行时解压缩。这会对内存使用产生影响，也会影响渲染速度。

DXT、PVRTC、ATC 和 ASTC 全都支持具有 Alpha 通道的纹理。这些格式还支持更高的压缩率和/或更好的图像质量，但仅在一部分 Android 设备上受支持。

可针对这些格式中的每种格式创建单独的 Android 分发存档 (.apk)，并让 Android Market 的过滤系统为不同的设备选择正确的存档。

## 电影/视频回放

我们建议您使用[视频播放器](https://docs.unity3d.com/Manual/VideoPlayer.html)播放视频文件。该组件取代了早期的电影纹理功能。

---

* <span class="page-history">在 Unity 5.6 中添加了视频播放器组件</span>
