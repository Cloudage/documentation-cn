# Vuforia 平台配置设置

本部分将介绍一些影响使用 Unity 开发 Vuforia 增强现实/混合现实应用程序的重要 XR 平台设置。

下面列出了 Vuforia Unity 集成中使用的重要 XR 设置。要访问这些设置，请打开 Player Settings（__Edit__ &gt; __Project Settings__ &gt; __Player__，然后选择目标构建设备的相应选项卡）：

* __Vuforia Augmented Reality support__：在应用程序中启用 Vuforia 增强现实支持

* __Virtual Reality SDKs__：允许将 Vuforia 功能集成到 VR 应用程序中。选中 Virtual Reality Supported 复选框时显示此菜单。

![XR Settings 中的 Vuforia Augmented Reality Support 和 Virtual Reality SDKs 列表](../uploads/Main/vuforia_ar_support.png)

Vuforia 支持以下 XR SDK：

* [Google Cardboard - Google VR SDK](https://developers.google.com/vr/cardboard/overview)

* [Google Daydream - Google VR SDK](https://developers.google.com/vr/cardboard/overview)

* [Microsoft HoloLens - Windows 10 SDK](https://developer.microsoft.com/en-us/windows/mixed-reality/install_the_tools)

* [Vuforia SDK](https://library.vuforia.com/content/vuforia-library/en/articles/Best_Practices/Best-practices-for-hybrid-VRAR-experiences.html)

__Vuforia VR SDK__（位于 __Virtual Reality SDKs__ 列表中）是一种没有外部依赖项的独立 VR 配置。该 SDK 提供立体渲染和失真校正、头部和手部跟踪（具有适用于 Tango 的位置跟踪）以及查看器配置文件支持（用于定义各种 VR 头盔的参数）。

## 立体渲染方法

立体渲染方法仅在使用 Vuforia 虚拟现实 SDK 时才有意义。Android、iOS 和 Windows Mixed Reality 设备支持多通道实例化、单通道实例化和非实例化。

将 Vuforia 增强现实与另一个虚拟现实 SDK 结合使用时，立体渲染支持由所使用的虚拟现实 SDK 决定。

## 支持的图形 API

__建议：__在 Unity 的 Player Settings（__Edit__ &gt; __Project Settings__ &gt; __Player__，然后选择目标构建设备的相应选项卡）中，打开 __Other Settings__，并启用 __Use Auto Graphics API__。


| __Android__| __iOS__ | __Windows__ |
|:---|:---|:---| 
| OpenGL ES 2.0<br/>OpenGL ES 3.x| OpenGL ES 2.0<br/>OpenGL ES 3.x <br/>Metal (iOS 8+) | Windows 10 上的 DirectX 11 |

## 支持的功能

Vuforia 提供了许多功能来协助开发 AR/MR 应用程序。

下表列出了这些功能以及支持它们的操作系统和设备类型。

| **功能**| **描述** | **操作系统** | **手机设备** | **眼镜** |
|:---|:---|:---|:---|:---| 
| 图像目标| 跟踪平面图像 | Android、iOS、UPW | 是 | 是 |
| 多个目标| 跟踪图像的几何排列 | Android、iOS、UPW | 是 | 是 |
| 圆柱体目标| 跟踪包裹在圆柱和圆锥上的图像 | Android、iOS、UPW | 是 | 是 |
| 用户定义的目标| 跟踪用户在运行时捕获的图像 | Android、iOS、UPW | 是 | 不推荐 |
| 云端识别| 基于云端的图像目标 | Android、iOS、UPW | 是 | 是 |
| 设备跟踪| 3 DoF 和 6 DoF 位置跟踪*  | Android、iOS、UPW | 是（仅限于 6 DoF） | 是（仅限于 6 DoF） |
| VuMark| 可定制、可编码的 AR 标记 | Android、iOS、UPW | 是 | 是 |
| 对象识别| 跟踪扫描中的 3D 对象 | Android、iOS、UPW | 是 | 是 |
| 模型目标**| 跟踪 3D 模型中的 3D 对象 | Android、iOS、UPW | 是 | 是 |
| 智能地形| 跟踪用户环境中的表面和几何体 | Android、iOS、UPW | 是 | 受限 |
| AR + VR| VR 应用程序的 AR 功能支持 | Android、iOS、UPW | 是 | 是 |

__*__ Tango 和 HoloLens 设备可使用 6 种自由度跟踪

__**__ 模型目标通过 Vuforia 早期访问计划 (Vuforia Early Access Program) 提供并在 Unity 2017.3 中公开

---
