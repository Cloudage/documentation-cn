# 核心事件

核心事件是基于会话和基于设备的 Analytics 事件。您为项目启用 Unity Analytics 时，Unity 会自动分发核心事件。核心事件为 Analytics 系统计算的多个指标提供基础信息，其中包括：

* 新安装 [New installs]
* 每日活跃用户 (DAU) [Daily active users (DAU)]
* 每月活跃用户 (MAU) [Monthly active users (MAU)]
* 会话总数 [Total sessions]
* 每个用户的会话数 [Sessions per user]
* 在应用程序中花费的时间 [Time spent in app]
* 按国家/地区和平台划分的用户细分段 [User Segments for country and platform]
* 收入（使用 Unity Ads 和/或 Unity IAP 时）

请参阅 [Analytics 指标、细分段和术语](UnityAnalyticsTerminology.html)以了解这些指标的定义。

核心事件包括：

* __AppStart__：在新会话开始时分发。一个新会话将在用户第一次启动应用程序时开始，或在应用程序处于非活动状态超过 30 分钟后将其切换到前台时开始。
* __AppRunning__：应用程序运行时定期分发。
* __DeviceInfo__：用户首次启动应用程序时以及每当设备信息变化时分发。

除了在 Analytics Dashboard 中查看基于核心事件的指标，您还可以使用 Raw Data Export 来下载事件。请注意，要使用 Raw Data Export，必须订阅 Unity Pro。

**注意：**Analytics 使用其他核心事件（如 __AppStop__、__AppUpdate__ 和 __AppInstall__）来计算指标，但不会通过 [Raw Data Export](UnityAnalyticsRawDataExport.html) 来显示这些指标。



---

* <span class="page-edit">2018-06-22 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">截至 2018-03-02，服务与 Unity 5.2 之后的版本兼容，但是版本兼容性可能会发生变化</span>

* <span class="page-edit">2018-06-04 - Removed UserInfo core event.</span>

* <span class="page-history">5.2 中的新功能</span>
