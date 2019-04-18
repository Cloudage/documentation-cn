Unity Remote 3（已弃用）
===========================

Unity Remote 3（也称为 Unity Remote）是一个应用程序，允许您将 iOS 设备用作 Unity 项目的远程控制装置。这在开发过程中非常有用，因为使用远程控制在编辑器中测试项目比在每次更改后构建和部署到设备更快。

已弃用
----------

这是与旧版本的 Unity Remote 相关的旧页面。请使用[最新版本的 Unity Remote](UnityRemote5.html)。

在何处可找到 Unity Remote？
------------------------------

可从 AppStore 免费下载 Unity Remote。如果您希望自行构建和部署应用程序，可从 Unity 网站的 [Unity 学习部分](http://unity3d.com/learn/resources/downloads)下载源代码。

如何构建 Unity Remote？
----------------------------


首先，从[此处](http://download.unity3d.com/unity/download/resources/UnityRemoteSource_3_5_x.zip)下载项目源代码，然后将其解压到所需位置。该 zip 文件包含一个 XCode 项目，用于构建 Unity Remote 并将其安装在您的设备上。

假设您已经创建资源调配配置文件并在设备上成功安装了 iOS 版本，则只需打开 Xcode 项目文件 UnityRemote.xcodeproj 即可。启动 XCode 后，应单击“Build and Go”以在 iOS 设备上安装该应用程序。如果您以前从未构建和运行过应用程序，我们建议您首先尝试构建一些 Apple 示例，以便熟悉 XCode 和 iOS。

安装 Unity Remote 后，请确保您的设备已通过 Wi-Fi 连接到同一局域网内的开发计算机，或者通过 USB 直接连接到计算机。在计算机上运行 Unity 的情况下，在 iPhone/iPad 上启动 Unity Remote，然后从显示的列表中选择您的计算机。现在，当 Editor 编辑器进入Play模式时，您的设备当作遥控器，可以使用它来开发和测试游戏。可以使用该设备来无线远程控制应用程序，还可以在设备屏幕上看到应用程序的低分辨率版本。

**注意：**Unity iOS 编辑器无法完美模拟设备的硬件，因此您可能无法获得与真实设备上完全相同的体验（图形性能、触摸响应、声音播放等）。

将 Unity Remote 部署到设备时，Xcode 显示奇怪的错误。我该怎么办？
---------------------------------------------------------------------------------------

表示 Unity Remote 项目中的默认标识符与您的资源调配配置文件不兼容。必须在 XCode 项目中手动更改此标识符。此标识符必须与资源调配配置文件匹配。

如果尚未创建带尾随星号的 AppID，则需要执行此操作；您可以在 Apple 的 iPhone Developer Program 的 Program Portal 中执行此操作。首先，访问 Program Portal，并选择 AppIDs 选项卡。然后，单击右上角的“添加 ID”(Add ID) 按钮，在 App ID Bundle Seed ID 和 Bundle ID (Bundle Identifier) 字段中输入常用的 Bundle ID，后跟点号和星号（例如，com.mycompany.*）。将新的 AppID 添加到资源调配配置文件，然后下载并重新安装该配置文件。不要忘记随后重新启动 Xcode。如果在创建 AppID 时遇到任何问题，请参考 Apple 网站上的[资源配置方法 (Provisioning How-to) 部分](http://developer.apple.com/iphone/manage/provisioningprofiles/howto.action)。


![在设备上安装 Unity Remote 之前，不要忘记更改标识符。](../uploads/Main/target_unity_remote_info.png)

使用 XCode 打开 Unity Remote 项目。从菜单中选择 __Project &gt; Edit Active Target "Unity Remote"__。随后将打开一个标题为 Target "Unity Remote" Info 的新窗口。选择 Properties 选项卡。将 Identifier 属性字段从 **com.unity3d.UnityRemote** 更改为 AppID 中的 Bundle ID，后跟“**.**”（点号）和“**UnityRemote**”。例如，如果资源调配配置文件包含 ######com.mycompany* AppID，则应将 Identifier 字段更改为 **com.mycompany.UnityRemote**。

接下来，从菜单中选择 __Build &gt; Clean all targets__，然后再次编译并安装 Unity Remote。您可能还需要将活动 SDK 从 Simulator 更改为 Device - 2.0 | Release。即使您的设备运行较新版本的操作系统，使用 SDK 2.0 也没有问题。

在 Unity Remote 中运行游戏时，图形质量非常差。如何做才能改善？
-----------------------------------------------------------------------------------------------------------

使用 Unity Remote 时，游戏实际上在您的 Mac 上运行，同时其可视内容通过深度压缩后串流到设备。因此，您在设备屏幕上看到的只是应用程序真实外观的低分辨率版本。您应该偶尔通过构建和部署应用程序（在 Unity Editor 中选择 __File &gt; Build & Run__）来检查游戏在设备上的运行情况。

Unity Remote 存在滞后。如何改善？
----------------------------------------

Unity Remote 的性能在很大程度上取决于 Wi-Fi 网络的速度、网络硬件的质量和其他因素。为获得最佳体验，请在 Mac 和 iOS 设备之间创建临时网络。单击 Mac 上的 Airport 图标，并选择“Create Network”。然后，输入名称和密码，并单击 OK。在设备上，选择 _Settings &gt; Wi-Fi_，并选择刚才新建的 Wi-Fi 网络。请注意，临时网络实际上是一种不涉及无线接入点的无线连接。因此，在使用临时网络时，通常无法访问 Internet。

在 iPhone/iPad 和 Mac 上关闭蓝牙应该也能提高连接质量。

如果不需要在设备上查看 Game 视图，可在远程计算机列表中关闭图像同步。这样做将减少 Remote 工作所需的网络流量。

与 Unity Remote 的连接很容易中断
---------------------------------------------

这可能是由安装问题或其他阻止 Unity Remote 正常运行的因素造成的。按顺序尝试以下步骤，在每个步骤检查性能是否有所改善，然后继续下一步：


1.首先，检查蓝牙是否已打开。为获得最佳性能，您的 Mac 和 iOS 设备都应禁用蓝牙。
1.删除位于 `~/Library/Preferences/com.unity3d.UnityEditoriPhone.plist` 的设置文件
1.在 iPhone/iPad 上重新安装游戏。
1.在 Mac 上重新安装 Unity。
1.作为最后的手段，对 iOS 设备执行硬重置有时可以提高 Unity Remote 的性能。

如果仍然遇到问题，请尝试在另一台设备上安装 Unity Remote（如果可能，请在另一个地点进行），查看是否能提供更好的结果。可能存在射频干扰问题或有其他软件影响 Mac 或 iOS 设备上的无线适配器性能。


Unity Remote 不能看到 Mac。我该怎么办？
--------------------------------------------------


* 检查 Unity Remote 和 Mac 是否连接到同一无线网络。
* 检查防火墙设置、路由器安全设置以及可能过滤网络数据包的任何其他硬件/软件。
* 让 Unity Remote 保持运行，关闭 Mac 的 Airport 一两分钟，然后重新打开。
* 重新启动 Unity 和 Unity Remote。有时还需要对 iPhone/iPad 执行冷启动（同时按住菜单按钮和电源按钮）。
* Unity Remote 会使用 Apple Bonjour 服务，因此请检查您的 Mac 是否已将其打开。
* 从最新的 Unity iOS 软件包重新安装 Unity Remote。
