# Android 清单

Android 清单是一个 XML 文件，其中包含有关 Android 应用程序的重要元数据。此文件包含包名称、活动名称、主活动（应用程序的入口点）、配置、Android 版本支持、硬件功能支持和权限。

有关 Android 清单文件的更多信息，请参阅有关 [Android 清单 (Android Manifests)](https://developer.android.com/guide/topics/manifest/manifest-intro.html) 的 Android 开发者文档。

## Unity 如何生成 Android 清单

在构建应用程序时，Unity 会按照以下步骤自动生成 Android 清单：

1.Unity 采用主 Android 清单。

2.Unity 查找插件（AAR 和 Android 库）的所有 Android 清单。

3.来自插件的清单通过 Google 的 [manifmerger](https://android.googlesource.com/platform/sdk/+/0386f5d/manifmerger) 类合并到主清单中。

4.Unity 修改清单，自动向清单添加权限、配置选项、使用的功能以及其他信息。

## 覆盖 Android 清单

尽管 Unity 会为您生成正确的清单，但在某些情况下，您可能希望直接控制其内容。

要使用您在 Unity 之外创建的 Android 清单，请将 Android 清单文件导入以下位置：_Assets/Plugins/Android/AndroidManifest.xml_。这将覆盖默认的由 Unity 创建的清单。

在这种情况下，Android 库的清单随后将合并到主清单中，得到的清单仍由 Unity 进行调整以确保配置正确。要完全控制清单（包括权限），必须导出项目并在 [Android Studio](https://developer.android.com/studio/index.html?gclid=CPbCm_HwptICFVXnGwodghYK5w) 中修改最终的清单。请注意，我们仅支持 [_launchMode - singleTask_](https://developer.android.com/guide/topics/manifest/activity-element.html#lmode)。

## 权限

Unity 会根据 [Player Settings](class-PlayerSettingsAndroid.html) 以及应用程序从脚本调用的 Unity API 自动为清单添加必要的权限。例如：

* [Network](../ScriptReference/Network.html) 类添加 `INTERNET` 权限

* 使用振动（例如 [Handheld.Vibrate](../ScriptReference/Handheld.Vibrate.html)）将添加 `VIBRATE`

* [InternetReachability](../ScriptReference/Application-internetReachability.html) 属性添加 `ACCESS_NETWORK_STATE`

* 位置 API（如 [LocationService](../ScriptReference/LocationService.html)）添加 `ACCESS_FINE_LOCATION`

* [WebCamTexture](../ScriptReference/WebCamTexture.html) API 添加 `CAMERA` 权限

* [Microphone](../ScriptReference/Microphone.html) 类添加 `RECORD_AUDIO`

有关权限的更多信息，请参阅 Android 开发者文档的 [Android 清单权限 (Android Manifest Permissions)](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) 页面。

请注意，如果插件通过在其清单中声明权限来请求权限，则在合并阶段会自动将权限添加到生成的 Android 清单中。插件调用的所有 Unity API 也会对权限列表有影响。

## Android 6.0 (Marshmallow) 中的运行时权限

如果应用程序在 Android 6.0 (Marshmallow) 或更高版本的设备上运行，并且还以 Android API 级别 23 或更高级别为目标，则该应用程序会使用 [Android 运行时权限系统 (Runtime Permission System)](https://developer.android.com/training/permissions/requesting.html)。

Android 运行时权限系统要求应用程序的用户在应用程序运行时授予权限，而不是在首次安装应用程序时授予权限。当应用程序运行时，应用程序用户通常可以在应用程序需要权限时授予或拒绝每个权限（例如，在拍照前请求相机权限）。这样可让应用程序在没有权限的情况下以有限的功能运行。

Unity 不支持运行时权限系统，因此应用程序在启动时会提示用户允许 Android 所谓的“危险”权限。请参阅有关[危险权限](https://developer.android.com/guide/topics/permissions/requesting.html) 的 Android 文档以了解更多信息。

提示用户允许危险权限是确保插件在缺少权限时不会导致崩溃的唯一方法。但是，如果不希望 Unity Android 应用程序在启动时请求权限，可将以下代码添加到清单中的 __Application__ 或 __Activity__ 部分。

```
<meta-data android:name="unityplayer.SkipPermissionsDialog" android:value="true" /> 

```

添加此代码会完全禁止启动时显示权限对话框，但您必须小心处理运行时权限以避免崩溃。此方法仅适用于高级 Android 应用程序开发者。

有关运行时权限系统和权限处理的更多信息，请参阅 Android 开发者文档的[请求权限 (Requesting Permissions)](https://developer.android.com/training/permissions/requesting.html) 部分。

## 检查生成的 Android 清单

要检查 Unity 为应用程序生成的最终 Android 清单，请在构建项目之后但在退出 Unity Editor 之前打开 _Temp/StagingArea/AndroidManifest.xml_ 文件。

清单以二进制格式存储在输出包 (APK) 中。要检查 APK 中的清单内容，可使用 [Android Studio APK Analyzer](https://developer.android.com/studio/build/apk-analyzer.html) 或其他第三方工具（例如 [Apktool](https://ibotpeaches.github.io/Apktool/)）。


----
* <span class="page-edit">2017-05-25 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.5 版中的更新功能</span>
