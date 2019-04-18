Oculus
======

<!-- https://trello.com/c/Qw7imxOL --> 

##准备开始 (Windows)

请参阅 [Oculus 开发者网站](https://developer.oculus.com)上的详细文档。特别要注意的是，请按照 [Unity 与 Oculus 结合使用](https://developer3.oculus.com/documentation/game-engines/latest/concepts/book-unity/)的说明进行操作，并查看关于 Oculus 开发的[建议规格](https://developer.oculus.com/documentation/game-engines/latest/concepts/unity-req/)。

##准备开始 (Gear VR)

无需在计算机上安装任何额外的程序即可部署到 Gear VR。请遵循 [Samsung 开发者网站](https://resources.samsungdevelopers.com/Gear_VR_and_Gear_360)上的说明。

确保可以将 Unity 应用程序部署到 Galaxy Note 5、S6 Edge +、S6 或 S6 Edge（请参阅 [Android 开发入门](android-GettingStarted.html)）：

1.用 micro USB 线缆将 Android 设备连接到 PC/Mac。
1.新建一个空项目（菜单：__File &gt; New Project__）。
1.将构建平台切换到 Android（菜单：File > Build Settings）。
1.打开 Player Settings（菜单：__Edit &gt; Project Settings &gt; Player__）。选择 Other Settings 并选中 Virtual Reality Supported 复选框。
1.将 Oculus 添加到 Virtual Reality SDK 列表。
1.在项目的 ``Assets`` 文件夹下创建文件夹 ``Plugins/Android/assets``（注意：此文件夹名称区分大小写）。
1.将一个 [Oculus 签名文件](https://developer.oculus.com/osig/)添加到项目的 ``Plugins/Android/assets`` 文件夹中。
1.构建并运行。将设备插入头盔，然后通过头部跟踪查看天空盒。

##准备开始 (OpenVR)

运行 OpenVR 应用程序需要 Steam，因此请安装 Steam 和 SteamVR。SteamVR 与头盔一起正常工作后，请将 OpenVR 添加到支持的 SDK 列表中。如果需要 Unity 内置支持之外的其他功能，请查看 [SteamVR Asset Store 资源包](https://www.assetstore.unity3d.com/en/#!/content/32647)。

##输入
请参阅有关 [Oculus 控制器](OculusControllers.html)的文档以查看输入控制映射。




