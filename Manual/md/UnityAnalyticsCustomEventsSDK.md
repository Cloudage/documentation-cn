自定义事件
=============

自定义事件可以是用户在游戏内执行的任意特定操作。自定义事件允许您跟踪 Unity Analytics 不会自动跟踪的玩家行为，例如关卡成就、场景更改、进入商店或与游戏对象交互。每个自定义事件都可以有自己的参数。设置关于事件的参数可让您过滤事件发生时收集的数据。在 Analytics Dashboard 中可以查看自定义事件的可视化工具，包括 Data Explorer、Funnel Analyzer 和 Segment Builder。

````
// 引用 Unity Analytics SDK 命名空间
using UnityEngine.Cloud.Analytics;

// 在玩家触发自定义事件的任意位置使用此调用
UnityAnalytics.CustomEvent(string customEventName,
IDictionary<string, object> eventData);
````

|**_UnityAnalytics.CustomEvent Input Parameters_**|||
|**_Name_** |**_Type_** |**_Description_** |
|:---|:---|
|__customEventName__ |string |Name of custom event. Name cannot include the prefix "unity." --- This is a reserved keyword. |
|__eventData__ |dictionary |Additional parameters sent to Unity Analytics at the time the custom event was triggered. eventData key cannot include the prefix "unity." --- This is a reserved keyword. |

关于自定义事件的一些注意事项：

* 务必保持一致！为事件数据中的每个参数维持一致的数据类型。例如，不要以数字形式发送关卡参数，然后又将其更改为字符串。这样做可能会导致错误的行为，使您的数据难以解读。
    * 布尔值 (true/false)
    * 字符串（字符）
    * 数字（int、float 等）。
* 每个自定义事件默认限制为不超过 10 个参数。
    * 如果传递的参数更多，则该调用将失败，返回值为 AnalyticsResult.TooManyItems
* 字典内容默认限制不超过 500 个字符。
    * 如果传递的字符超过 500 个，则该调用将失败，返回值为 AnalyticsResult.SizeLimitReached
* 每个用户每小时默认限制为不超过 100 个自定义事件。
    * 如果每小时调用的事件超过 100 个，则该调用将失败，返回值为 AnalyticsResult.TooManyRequests
* 注意 Analytics 系统如何解析参数。
    * 所有数字（int、float 等类型）即使以字符串形式发送也会解析为数字。
    * 只将字符串和布尔值视为“可分类”。
    * 因此，如果希望对某些数字求和或求平均值，请将其以数字形式（例如，51 或 '51'）发送。如果希望将其分类（就像对关卡或选项所做的操作一样），请确保将其解析为字符串（例如，'Level51'）。

在以下示例中，我们希望了解游戏结束时用户在库存中拥有的商品。


````
// 引用 Collections Generic 命名空间
  using System.Collections.Generic;

  int totalPotions = 5;
  int totalCoins = 100;
  string weaponID = "Weapon_102";
  UnityAnalytics.CustomEvent("gameOver", new Dictionary<string, object>
  {
    { "potions", totalPotions },
    { "coins", totalCoins },
    { "activeWeapon", weaponID }
  });
````

按 Play
----------
要将测试自定义事件数据发送到服务器并验证您的集成情况，请在 Editor Play 模式下触发您的自定义事件。

![](../uploads/Main/AnalyticsPlayGame.gif) 

