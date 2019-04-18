# 自定义事件脚本

作为使用 [AnalyticsEventTracker](class-AnalyticsEventTracker.html) 组件的替代方法，也可以通过脚本调用 [AnalyticsEvent.Custom](Scriptref:AnalyticsEvent.Custom.html) 来直接发送自定义事件。

```
// 引用 Unity Analytics 命名空间
using UnityEngine.Analytics;

// 在玩家触发自定义事件的任何位置使用此调用
AnalyticsEvent.Custom(string customEventName, IDictionary<string, object> eventData);
```

## Analytics.CustomEvent 输入参数

|**_Name_** |**_Type_** |**_Description_** |
|:---|:---|
|__customEventName__ |string |Name of the custom event. The name cannot include the prefix "unity", which is reserved for internal Unity Analytics events. |
|__eventData__ |dictionary |Additional parameters sent to Unity Analytics at the time the custom event was triggered. eventData keys cannot include the prefix "unity", which is reserved for internal Unity Analytics events.|

以下示例在玩家在关卡中找到秘密位置时发送自定义事件：

```
public void ReportSecretFound(int secretID){
    AnalyticsEvent.Custom("secret_found", new Dictionary<string, object>
    {
        { "secret_id", secretID },
        { "time_elapsed", Time.timeSinceLevelLoad }
    });
}
```

---

* <span class="page-edit">2018-03-02 Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">截至 2018-03-02，服务与 Unity 5.2 之后的版本兼容，但是版本兼容性可能会发生变化</span>

* <span class="page-history">5.2 中的新功能</span>

* <span class="page-history">标准事件可作为 Unity 插件包添加进来。</span>
 
