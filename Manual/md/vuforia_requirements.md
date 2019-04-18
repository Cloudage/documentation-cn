# Vuforia 硬件和软件要求

本页面将概述 Vuforia SDK 与 Unity 结合使用的硬件和软件要求。

## 移动设备

为了运行使用 Vuforia SDK 创建的应用程序，需要采用支持的移动设备操作系统 (OS) 版本，下表列出了这些版本。该表还包括使用 Vuforia 开发应用程序时支持的操作系统和 Unity 版本。

| **设备操作系统**|  | **开发操作系统**|  | **Unity 版本**|  |
|:---|:---|:---|:---|:---|:---| 
| Android (1)| 4.1.x+ | Windows (2) | 7+ | Windows (2) | 2017.2+ |
| iOS (2) | 9+ | OS X | 10.11+ | OS X | 2017.2+ |
| Windows (2)| 10 UWP |  |  |  |  |

1.仅 32 位
2.32 位和 64 位

## 光学透视数码眼镜

Vuforia SDK 支持一系列光学透视数码眼镜设备。下表列出了支持的设备及其所需的操作系统 (OS) 版本。该表还包括使用 Vuforia 为这些设备开发应用程序时支持的操作系统和 Unity 版本。

| **设备**|  | **设备操作系统** |  | **开发操作系统** |  | **Unity 版本** |  |
|:---|:---|:---|:---|:---|:---|:---|:---| 
| HoloLens| 当前版本 | Android (1)(2) | 4.0.3+ | Windows (3) | 7+ | Windows (3) | 2017.2+ |
| ODG| R7+ | Windows (1) | 10 UWP | OS X | 10.11+ | OS X   | 2017.2 |
| Epson| BT-200 |  |  |  |  |  |  |

1.仅 32 位
2.4.0.3 Native 仅支持 Epson BT-200
3.32 位和 64 位


## Vuforia 工具

有些 Vuforia 工具仅在特定硬件上受支持。下表列出了这些工具及其支持的设备和所需的操作系统版本。

| **应用程序**| **设备** | **操作系统版本** |
|:---|:---|:---| 
| Calibration Assistant| Moverio BT-200<br/>ODG R-7 | Android 4.0.3+ |
| Object Scanner| Samsung Galaxy S8+<br/>Samsung Galaxy S8 <br/>Samsung Galaxy S7<br/>Samsung Galaxy S6 | 设备上支持的最新操作系统 |

有关这些工具的更多详细信息，请参阅 [Vuforia 开发者库](https://library.vuforia.com/)中的 [Vuforia Calibration Assistant](https://library.vuforia.com/articles/Training/Vuforia-Calibration-App) 和 [Object Scanner](https://library.vuforia.com/articles/Training/Vuforia-Object-Scanner-Users-Guide) 页面。

### 虚拟现实 SDK 集成支持

下面列出了完全集成到 Unity Editor 中的 VR SDK 以及首次集成它们的 Editor 发行版本。

| __VR SDK__| __版本__ |
|:---|:---| 
| Google VR SDK| Unity 2017.2 |
| Cardboard Android SDK| Unity 2017.2 |
| Windows Mixed Reality（仅限 HoloLens）| Unity 2017.2 |

### 图形 API 支持

下表列出了在支持的操作系统上开发应用程序时 Vuforia 支持的所有图形 API。

| __Android__| __iOS__ | __Windows__ |
|:---|:---|:---| 
| OpenGL ES 2.0<br/>OpenGL ES 3.x| OpenGL ES 2.0<br/>OpenGL ES 3.x <br/>Metal (iOS 8+) | Windows 10 上的 DirectX 11 |

* 有关将 Vuforia 与数码眼镜（例如 ODG R7+ 和 Epson BT-200）结合使用的更多信息，请参阅关于 [Vuforia 数码眼镜应用](https://library.vuforia.com/content/vuforia-library/en/articles/Training/Vuforia-for-Digital-Eyewear.html)的 Vuforia 文档

* 有关将 Vuforia 与 HoloLens 结合使用的更多信息，请参阅关于[使用 Unity 中的 Hololens 示例](https://library.vuforia.com/content/vuforia-library/en/articles/Solution/Working-with-the-HoloLens-sample-in-Unity.html)的 Vuforia 文档

* 有关将 Vuforia 与 Google VR SDK 结合使用的更多信息，请参阅关于 [Google Cardboard 开发](https://library.vuforia.com/content/vuforia-library/en/articles/Solution/Developing-for-Google-Cardboard.html)的 Vuforia 文档

---
* <span class="page-edit">2018-03-28 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 2017.3 版中更新了有关 Unity XR API 的 Vuforia 文档</span>
