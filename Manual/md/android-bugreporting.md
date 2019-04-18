# 报告 Android 下的崩溃错误

在提交错误报告之前，请参阅 Unity 手册的 [Android 开发故障排除](TroubleShootingAndroid.html)页面和 [Unity Android 论坛](https://forum.unity3d.com/forums/android.30/)，获取常见崩溃和问题的解决方案。

调查 Android 上的崩溃问题可能极具挑战性，特别是您无法在自己的设备上重现崩溃的情况下。可使用 [Android Debug Bridge (ADB)](https://developer.android.com/studio/command-line/adb.html) 通过运行 `adb logcat` 来创建设备的日志。如需了解 ADB logcat 文件的内容，请参阅官方 Android Studio 文档的 [logcat 命令行工具](https://developer.android.com/studio/command-line/logcat.html)页面。

如果无法通过上述方法找到问题的解决方案，请提交错误报告，并在提交之前将 ADB logcat 文件和完整的 Unity 项目附加到错误报告中，同时应遵循 [Unity 错误报告指南](https://unity3d.com/unity/qa/bug-reporting.)。

----
* <span class="page-edit">2017-05-25 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.5 版中的更新功能</span>
