#AAR 插件和 Android 库

##AAR 插件

Android Archive (AAR) 插件是包含已编译的 Java 代码和本机 (C/C++) 代码、资源以及 Android 清单的捆绑包。.aar 文件本身是一个包含所有资源的 zip 存档。有关更多详细信息，请参阅 Android 开发者文档的[创建 Android 库](https://developer.android.com/studio/projects/android-library.html#aar-contents)部分。

要将 AAR 插件添加到项目中，请将 .aar 文件复制到项目的任意文件夹中，然后在 Unity 中选择该文件，从而在 Inspector 窗口中打开 Import Settings。应勾选 __Android__ 复选框以将此 .aar 文件标记为与 Unity 兼容：


![Inspector 窗口中显示的 ARR 插件导入设置](../uploads/Main/AndroidARRPlugins.png)

AAR 是建议用于 Unity Android 应用程序的插件格式。


##Android 库项目
Android 库项目类似于 AAR 插件：它们包含本机代码和 Java 代码、资源以及 Android 清单。但是，Android 库不是单个存档文件，而是一个包含所有资源的特殊结构目录。有关更多详细信息，请参阅 Android 开发者文档的[创建 Android 库](https://developer.android.com/studio/projects/android-library.html#aar-contents)部分。


将预编译的 Android 库项目导入 Assets/Plugins/Android 文件夹。预编译意味着所有 .java 文件在导入 Unity 之前必须已编译为 .jar 文件并放置在 Android Studio 项目的 bin/ 或 libs/ 文件夹中。从这些文件夹中，AndroidManifest.xml 会在项目构建时自动与主清单文件合并。

Unity 会将 Assets/Plugins/Android 的所有子文件夹视为潜在的 Android 库，并会禁止从这些子文件夹中导入资源。如果子文件夹中包含 AndroidManifest.xml 文件，而 project.properties 文件中包含字符串 `android.library=true`，则该子文件夹将被识别为 Android 库。

请参阅 Android 开发者文档的[库模块](https://developer.android.com/studio/projects/index.html#ApplicationModules)部分以了解更多详细信息。

##提供额外的 Android 资产和资源

如果需要将资源添加到 Unity 应用程序，并且应将这些资源按原状复制到输出包中，请将它们导入 Assets/Plugins/Android/assets 目录。这些资源会出现在 APK 的 assets/ 目录中，并可使用 Java 代码中的 [getAssets()](https://developer.android.com/reference/android/content/res/Resources.html#getAssets()) Android API 访问它们。

<br/> 

----
* <span class="page-edit">2017-05-18  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.5 版中的更新功能</span>
