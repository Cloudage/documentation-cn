# 自定义事件

自定义事件可以是用户在游戏内执行的任意特定操作。此类事件可用于跟踪 Unity Analytics 没有自动跟踪且没有相关标准事件的玩家行为。每个自定义事件最多可以有十个参数。没有必需参数。

一般情况下，仅在没有定义适合的标准事件时才应使用自定义事件。因为标准事件具有明确的上下文，所以 Unity 可以提供比自定义事件更好的分析和可视化支持。

可使用 [Analytics Event Tracker](class-AnalyticsEventTracker.html) 组件将自定义事件发送到 Analytics 服务。直接选择__自定义__事件而不是标准事件。此外也可以[使用代码](UnityAnalyticsCustomEventScripting.html)来发送自定义事件。

---

* <span class="page-edit">2018-03-02 Page published with no [editorial review](DocumentationEditorialReview.html)
</span>
<span class="page-edit">截至 2018-03-02，服务与 Unity 5.2 之后的版本兼容，但是版本兼容性可能会发生变化</span>

* <span class="page-history">5.2 中的新功能</span>

* <span class="page-history">标准事件可作为 Unity 插件包添加进来</span>
 

