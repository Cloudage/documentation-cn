# OpenVR

## 准备开始 (OpenVR)
<!-- https://trello.com/c/Qw7imxOL --> 

运行 OpenVR 应用程序需要 Steam，因此请安装 Steam 和 SteamVR。SteamVR 与头盔一起正常工作后，请将 **OpenVR** 添加到支持的 SDK 列表中（请参阅下文以了解如何执行此操作）。如果需要 Unity 内置支持之外的其他功能，请查看 [SteamVR Asset Store 资源包](https://www.assetstore.unity3d.com/en/#!/content/32647)。

##支持的平台
以下平台支持 OpenVR：

* Windows
* macOS

###特定于 macOS 的注意事项

* macOS 上的 Unity OpenVR 需要 Metal 图形和 64 位应用程序目标，不支持 OpenGL。

* OpenVR 支持 macOS 10.11.6 或更高版本，但针对 macOS 10.13 High Sierra 或更高版本进行了优化。

##输入
请参阅有关 [OpenVR 控制器](OpenVRControllers.html)的文档以查看输入控制映射。

## 升级包含 SteamVR 资源包的项目

* 启用虚拟现实支持。打开 __Player Settings__（菜单：__Edit__ > __Project Settings__ > __Player__），选择 __Other Settings__，然后选中 __Virtual Reality Supported__ 复选框。
* 使用该复选框下方显示的 __Virtual Reality SDK__ 列表添加 OpenVR。
* 在 Editor 中进入播放模式来测试构建结果。

---
* <span class="page-edit">2017-08-04  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>


* <span class="page-history">在 [2017.2](https://docs.unity3d.com/2017.2/Documentation/Manual/30_search.html?q=newin20172) 版中更新了支持的平台 <span class="search-words">NewIn20172</span></span>


