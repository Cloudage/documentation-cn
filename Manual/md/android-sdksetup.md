# Android 环境设置

无论是在 Unity 中构建 Android 应用程序还是从头开始编程，都必须先设置 Android 软件开发工具包 (SDK)，然后才能在 Android 设备上构建并运行代码。

### 1.安装 Java 开发工具包
下载并安装 [Java 开发工具包 (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)。Unity 需要 64 位版本的 JDK 8 (1.8)。

### 2.下载 Android SDK

You can install the Android SDK using command line tools or through Android Studio. Android Studio provides an easy to use GUI based tool, but installs additional software on your computer. Using the command line tools is a smaller download and does not install additional software, but it can be more challenging to use.

#### 2a.使用命令行工具安装 Android SDK

安装或解压 Android SDK。安装完成后，打开 Android SDK Manager 并添加：至少一个 Android SDK 平台 (Android SDK Platform)、平台工具 (Platform Tools)、构建工具 (Build Tools) 和 USB 驱动程序（如果使用的是 Windows）。

要安装 Android 平台 SDK 和相关工具，请执行以下操作：

1.下载 [Android 软件命令行工具](https://developer.android.com/studio/index.html#downloads)。

2.将 tools 文件夹解压缩到硬盘驱动器上的某个位置。

3.打开命令提示符窗口。

4.导航到 tools 文件夹的解压缩位置中的 bin 文件夹：
	"安装文件夹" > tools > bin

5.使用 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html) 命令行工具检索可安装包列表。可安装包中包括平台 SDK、构建工具、平台工具和其他工具。

    *sdkmanager --list*

6.选择要安装的平台 SDK 版本。平台 SDK 在列表中采用以下形式：platforms;android-xx。*xx* 表示 SDK 级别。数字越大，包越新。
通常，可安装最新的可用版本。但是，在某些情况下，Google 发布了新版本的 SDK，但在构建 Unity 项目时会导致错误。在这种情况下，必须卸载 SDK 并安装早期版本。
包安装命令的一般格式为 *sdkmanager &lt;包名称&gt;*。可同时安装相应的平台工具和构建工具。


	示例：
		*sdkmanager "platform-tools"  "platforms;android-27" "build-tools;27.0.3"*

1.如果是在 Windows 上运行，请安装 USB 设备驱动程序。

	*sdkmanager "extras;google;usb_driver"*

这会将 SDK 安装在先前解压缩 tools 文件夹所在的目录中名为“platforms”的目录中。
示例：
c:\&lt;安装文件夹&gt;\platforms

#### 2b.使用 Android Studio 安装 SDK

从 [Android 开发者门户](https://developer.android.com/index.html)安装 Android Studio。Android 开发者门户提供了详细的安装说明。

**Note**: Android Studio provides some ease of use benefits, but it is not fully tested for compatibility with Unity installs. If you encounter errors, Unity recommends using the command line method.

安装 Android 平台 SDK 和其他工具时，通常可安装最新的可用版本。在某些情况下，Google 发布了新版本的 SDK，但在构建 Unity 项目时会导致错误。在这种情况下，必须卸载 SDK 并安装早期版本。

应同时安装相关的平台工具和构建工具。如果是在 Windows 上运行，请安装 USB 设备驱动程序。

### 3.在设备上启用 USB 调试

要启用 USB 调试，必须在设备上启用开发者选项 (Developer options)。要完成该操作，请在您的设备的 __Settings__ 菜单中找到版本号。版本号的位置因设备而异。可通过导航到 __Settings__ > __About phone__ > __Build number__ 来找到初始 Android 设置。有关设备和 Android 版本的具体信息，请咨询硬件制造商。

![在 Samsung Galaxy Note 3 上的 Android 5.0 (Lollipop) 中显示的 Build number](../uploads/Main/android-sdksetup-0.png)

__注意：__在 4.2 (Jelly Bean) 之前的 Android 版本上，不会隐藏 Developer options。选择 __Settings__ > __Developer options__，然后启用 USB debugging。

按照上述说明导航到版本号后，点击版本号七次。随后会弹出一条通知消息“You are now X steps away from being a developer”，其中“X”是一个数字，每点击一次就会倒数一个数。在第七次点击时，Developer options 将解锁。

用 USB 线缆将设备连接到计算机。如果在 Windows 计算机上进行开发，可能需要安装特定于设备的 USB 驱动程序。如需了解更多信息，请参阅设备制造商网站。

Windows 和 macOS 的设置过程有所不同；对此，Android 开发者网站上有详细说明。有关将 Android 设备连接到 SDK 的更多信息，请参阅 Android 开发者文档的[运行应用程序 (Running Your App)](https://developer.android.com/training/basics/firstapp/running-app.html#Emulator) 部分。

在设备通过 USB 连接到计算机时，选择 __Settings__ > __Developer options__，并选中 USB debugging 复选框以启用调试模式。

![在 Android 5.0 (Lollipop) 中显示的 Developer options - Samsung Galaxy Note 3](../uploads/Main/android-sdksetup-1.png)

### 4.在 Unity 中配置 Android SDK 路径

第一次创建 Android 项目时（或者如果 Unity 稍后无法找到 SDK），Unity 会要求您找到安装 Android SDK 的文件夹。

如果是使用的 sdkmanager 来安装 SDK，则可在 &lt;android 工具安装位置&gt;\platforms\&lt;android sdk 文件夹&gt; 中找到该文件夹。

示例：

*c:\&lt;android 工具安装位置&gt;\platforms\android-27*

如果是在安装 Android Studio 时安装的 SDK，则可在 Android Studio SDK Manager 中找到该位置。要从 Android Studio 打开 SDK Manager，请单击 __Tools > Android > SDK Manager__ 或单击工具栏中的 __SDK Manager__。

![SDK Manager 工具栏按钮](../uploads/Main/androidsdkmanagertoolbarbutton.png)

要更改 Android SDK 的位置，请在菜单栏中选择 __Unity__ > __Preferences__ > __External Tools__。

### 5.下载并设置 Android NDK

如果要使用 Android 的 [IL2CPP](https://docs.unity3d.com/Manual/IL2CPP.html) 脚本后端，则需要 Android 原生开发工具包 (NDK)。该工具包中包含构建必要库并最终生成输出包 (APK) 所需的工具链（如编译器和链接器）。如果不以 IL2CPP 后端为目标，则可以跳过此步骤。

从 [NDK 下载](https://developer.android.com/ndk/downloads/index.html)网页下载 Android NDK 版本 r13b（64 位）。将 *android-ndk* 文件夹解压缩到计算机上的目录，并记下该位置。

第一次使用 IL2CPP 构建 Android 项目时，系统会要求您找到安装 Android NDK 的文件夹。选择 NDK 安装的根文件夹。要更改 Android NDK 的位置，请在 Unity Editor 中导航到菜单 __Unity__ > __Preferences__ 以显示 Unity Preferences 对话框。在此处单击 __External Tools__。

