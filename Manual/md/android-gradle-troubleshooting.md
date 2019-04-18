<!-- Anchor tags on this page are linked to from within the Unity Editor --> 

# Gradle 故障排除

如果刚从旧系统切换到 Gradle 并从新系统导出 Android 项目，可能会遇到构建错误，尤其是在使用其他 Android 库或者添加了自定义 __AndroidManifest.xml__ 的情况下。

Android Gradle 插件远比旧的 ADT/Ant 系统更挑剔。它不接受
任何它认为有错的内容，无论是重复的符号、对不存在资源的引用
还是与主应用程序设置相同属性的库项目。

在大多数情况下，修复此类问题涉及编辑 __AndroidManifest.xml__ 文件；要么是主文件，
要么是项目使用的库中的文件。

在不寻常的项目中，或者如果项目有下方故障排除部分未描述的问题，
请将项目导出为 Gradle 项目（通过 __Build Settings__）并从命令行进行构建。从命令行进行构建可提供更详细的错误消息，并在应用更改时提供更短的周转时间。

## 特定问题

### 找不到资源 [resource-not-found]

一个 __AndroidManifest.xml__ 文件（主文件或库中的文件）引用不存在的
资源。通常，该资源是由库设置的应用程序图标或标签字符串。如果
已将主清单复制到库项目但未删除这些引用，则会发生这种情况。

从其中一个 Android 清单（通常是库中的清单）中删除该属性。

### APK 中出现重复文件 [duplicate-files-in-apk]

主应用程序和库项目之间或两个库项目之间存在
文件名冲突。请注意，所有文件都会复制到同一 APK 包中。

需要删除其中一个文件。

### 包名称冲突 [colliding-package-names]

库不能使用与主应用程序或任何其他库相同的 Java 包。

通常，应将库的包名称更改为其他不同的名称。如果库
包含大量代码，可能更容易更改主包名称（通过 _Player Settings_）。

### 属性冲突 [colliding-attributes]

库无法自由覆盖主 __AndroidManifest.xml__ 中的属性。通常，此错误是由设置应用程序图标或标签字符串的库引起的，类似于上面的__找不到资源__问题。

从库中删除属性，或者将 __tools:replace__ 属性添加到
__application__ 标签以指示应如何解决合并冲突。

<!-- Add empty lines so that web page can be positioned with linked header on top -->
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
