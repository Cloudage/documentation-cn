# Analytics 事件

在有人使用您的应用程序或游戏时，Unity 通过分发事件来收集 Analytics 数据。Unity 自动分发一组核心事件。您可以分发更多事件来收集有关用户如何玩游戏或使用应用程序的更丰富和更深入的信息。

Analytics 事件类型包括：

* [核心事件](UnityAnalyticsCoreEvents.html)：基于会话和基于设备的事件。
* [标准事件](UnityAnalyticsStandardEvents.html)：具有标准名称和参数的行为事件，用于跟踪用户体验和玩家行为。
* [自定义事件](UnityAnalyticsCustomEvents.html)：可完全自定义的事件，您可以根据需要定义其名称、用途和参数。
* [交易事件](UnityAnalyticsMonetization.html)：此类专用事件可用于跟踪通过第三方 IAP API 进行的应用内购 (IAP) 交易。
* 其他：诸如[热图 (Heatmap)](https://bitbucket.org/Unity-Technologies/heatmaps/wiki/v2.md) 事件之类的专用事件，用于特定目的。此类事件通常基于自定义事件，但用于发送它们的 API 将控制事件名称和参数。

Unity 自动分发核心事件。您必须在适当时间分发其他类型的事件。可从 [Analytics Event Tracker 组件](class-AnalyticsEventTracker.html)或从脚本分发标准事件和自定义事件。

Unity 游戏或应用程序会将 Analytics 事件发送到 Unity Web 服务。Analytics 系统收集原始事件并处理它们以便显示在项目的 Analytics Dashboard 上。如果运行应用程序的计算机或设备无法连接到 Web 服务，Unity 会在本地缓存事件，直到连接可用。

注意：如果已订阅 Unity Pro，可直接[下载原始事件数据](UnityAnalyticsRawDataExport.html)。您可以将此原始数据导入到自己的分析工具中，并执行 Unity Analytics Dashboard 可能不支持的特定分析。

## 标准事件与自定义事件

标准事件是自定义事件的一种形式。不同之处在于标准事件已标准化。每个标准事件都有一个预定义事件名称和一组必需参数。标准化带来了若干好处：

* 有助于避免易犯的编码错误，例如为本应该是同一事件的事件使用略微不同的名称和参数。
* 引导在应用程序中的有用位置分发 Analytics 事件。
* 为事件提供已知上下文，使 Unity Analytics 系统能够使用该上下文来提供更有意义的 Analytics 报告。

鉴于有这些好处，应尽可能使用标准事件而不是自定义事件。只有在没有合适的标准事件时才应使用自定义事件。

---

* <span class="page-edit">2018-03-02 Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">2018-06-04 - Service compatible with Unity 5.2 onwards at this date but version compatibility may be subject to change</span>

* <span class="page-history">5.2 中的新功能</span>

* <span class="page-history">2018-06-04 - Demographics event removed</span>
