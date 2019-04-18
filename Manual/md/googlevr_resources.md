# Google VR 资源和故障排除

## 实用链接

下面列出了一些有助于进一步了解 Google VR 开发主题的有用外部链接。

* [Google 开发者网站上的 VR 部分](https://developers.google.com/vr/)

* [Cocoapods](http://cocoapods.org)

## 故障排除指南

本部分将列出使用 Google VR SDK 进行开发时可能遇到的最常见问题。下表列出了一些常见问题以及建议的解决方案。

| __问题__| __建议__ |
|:---|:---| 
| 我有 GvrController/GvrViewer/GvrXXXX/Instant Preview 方面的问题。如何解决此问题？<br/>| 这些类型是 Google VR SDK for Unity 的一部分，由 Google 拥有并提供支持。虽然一般的 Unity 社区可能也能够回答您的问题，但对于任何技术问题，应该访问 GitHUB 上的 [Google VR SDK for Unity](https://github.com/googlevr/gvr-unity-sdk) 站点。<br/> |
| Daydream Keyboard 什么时候发布？| 适用于 Google VR 的 Daydream Keyboard 是 Google 技术，将在未来某个核心 Android 系统版本中发布。此技术的访问和使用完全基于 Google 和 [Google VR SDK for Unity](https://github.com/googlevr/gvr-unity-sdk)。<br/> |
| 我正在尝试构建以 iOS 为目标的 Cardboard 项目，但 Xcode 报告了链接器错误。| Google 通过 Cocoapods 库管理系统来分发适用于 iOS 的 Cardboard 原生开发工具包 (NDK)。Unity 与 Cocoapods 管理器中特定版本的 Cardboard NDK 集成，并使用该 NDK 创建您的 XCode 项目。生成的项目与标准 Unity 项目的生成方式不同。Cocoapods 创建一个包含 Unity 项目的 XCode 包装工作空间，并创建一个用于 Cardboard NDK 库及其依赖项的项目。请始终确保打开和/或使用该工作空间而不仅仅是项目本身，从而避免由于缺少 Cocoapods 中的库而导致链接器错误。 |



请参阅官方的 Google VR 开发者网站，了解有关更多有关 [Cardboard](https://support.google.com/cardboard/answer/6295070?hl=en&ref_topic=6295055) 和 [Daydream](https://support.google.com/daydream/?hl=en-GB#topic=7184600) 的故障排除信息。

---
* <span class="page-edit">2018-03-27 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 2017.3 版中更新了有关 Unity XR API 的 Google VR 文档</span>
