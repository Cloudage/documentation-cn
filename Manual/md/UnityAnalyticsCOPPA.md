# COPPA 合规性

COPPA 表示儿童网络隐私保护法（和相关法规）。COPPA 规定了有关“面向 13 岁以下儿童”的应用程序（“儿童应用程序”）运营商的某些义务。如果您对自己的应用程序是否为儿童应用程序或在 COPPA 的其他方面有疑问，应参考美国联邦贸易委员会提供的材料（此处提供了其部分内容：[COPPA FAQ](https://www.ftc.gov/tips-advice/business-center/guidance/complying-coppa-frequently-asked-questions)）并咨询您自己的法律顾问。

Unity Analytics 对从指定为儿童应用程序的应用程序收集的数据的处理不同于从其他应用程序收集的数据的处理。如果您的应用程序是儿童应用程序，则需要在 Editor 服务面板内通过 [Unity Ads 开发者控制面板 (Publisher Dashboard)](https://dashboard.unityads.unity3d.com/) 中的项目创建过程或者通过 [Unity Ads 开发者控制面板 (Publisher Dashboard)](https://dashboard.unityads.unity3d.com/) 中的项目概述页面将应用程序指定为儿童应用程序。

 为了提供游戏的分析数据，Unity Analytics 会为游戏中的每个用户生成一个匿名用户 ID。我们不使用从儿童应用程序生成的任何 ID 来跟踪其他开发者构建的应用程序中的用户，也不会使用这些 ID 在不同服务、设备或同一台计算机上的浏览器之间映射用户。除了这些 ID，Unity Analytics 还从儿童应用程序用户收集以下个人信息：IP 地址、广告商标识符（仅在同时启用 Unity Ads 的情况下收集 IDFA）和设备标识符（IDFV、Android 设备 ID 或 IMEI - 如果 Android 设备 ID 不可用）。

Unity 对以上所述的个人信息的使用仅限于为应用程序开发者提供应用程序级别的分析，以及分析和报告有关设备、应用程序和游戏行业的匿名和汇总级别信息（例如，使用特定操作系统的设备的百分比或某些设备按地区划分的百分比）。此汇总级别的数据不包含个人信息。此外，如果已在儿童应用程序中启用 Unity Ads，则 Unity 可能会使用 Unity Analytics 从该儿童应用程序收集的用户相关信息以便在该儿童应用程序内提供上下文广告。

---
* <span class="page-edit">2017-08-29  Page published with [editorial review](DocumentationEditorialReview.html)
</span>
