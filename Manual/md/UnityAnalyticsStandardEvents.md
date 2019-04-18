# 标准事件

使用标准事件可以在五个主要方面跟踪用户体验和玩家行为：

* __应用程序__ (Application)：跟踪应用程序用户界面中基本元素的使用情况
* __进度__ (Progression)：跟踪玩家在应用程序或游戏中的进度
* __入门__ (Onboarding)：跟踪玩家与应用程序或游戏最早进行的交互
* __参与度__ (Engagement)：跟踪与社交分享、成就相关的重要活动
* __变现__ (Monetization)：跟踪与收入相关的事件以及游戏内的经济情况

标准事件列表涵盖可用于跟踪游戏和应用程序的一系列内容。例如，大多数游戏都有玩家进度的概念，其中可能包括完成的猜谜、参与过的比赛、获得的 RPG 类经验或其他一些概念。通过跟踪标准进度事件，可更好地了解玩家停止进度的位置以及最终停止玩游戏的位置。

大多数标准事件都会定义必需参数和可选参数。这些参数可在发送 Analytics 事件时提供有关游戏状态的其他信息。您也可以定义自己的自定义参数。设置关于事件的参数可让您过滤事件发生时收集的数据。在 [Analytics Dashboard](UnityAnalyticsDashboard.html) 上可以查看标准事件和自定义事件的可视化工具，包括 Data Explorer、Funnel Analyzer 和 Segment Builder。

Unity Analytics 将它接收的标准事件和自定义事件的数量限制为每个用户每小时 100 个。换言之，如果玩游戏的单个用户设法在一小时内触发超过 100 个标准事件或自定义事件，Analytics 就会丢弃多余的事件。


**注意**：要在 Unity 2017.3 之前的版本中使用标准事件，首先需要从 Unity Asset Store 下载并导入 [Standard Events SDK](https://assetstore.unity.com/packages/add-ons/services/analytics/unity-analytics-standard-events-91846)。

## 应用程序事件

应用程序事件跟踪玩家在游戏本身之外与应用程序的交互（例如，在菜单中或者关卡之前或之后的过场动画屏幕中进行的交互）。通过分析应用程序事件可以查看玩家是否按照预期经常使用用户界面的基本部分。

| **事件名称** | **目的** |
| :--- | :--- |
| screen_visit | 玩家在 UI 中打开一个界面，比如高分或设置界面。 |
| cutscene_start | 玩家开始观看过场动画。 |
| cutscene_skip | 玩家跳过过场动画。 |

## 进度事件
进度事件跟踪玩家在游戏中的进度。分析标准进度事件可以监控玩家在游戏中的进度。

| **事件名称** | **目的** |
| :--- | :--- |
| game_start | 玩家开始新游戏（对于具有明显开头和结尾的游戏很有用）。 |
| game_over | 玩家结束游戏（对于具有明显开头和结尾的游戏很有用）。 |
| level_start | 玩家开始玩某个关卡。 |
| level_complete | 玩家成功完成一个关卡。 |
| level_fail | 玩家在某个关卡失败。 |
| level_quit | 玩家退出某个关卡。 |
| level_skip | 玩家跳过某个关卡。 |
| level_up | 玩家等级或 RPG 风格经验级别提高。 |

## 入门事件
入门事件跟踪首次用户体验 (FTUE)。分析标准入门事件可以监控教程的有效性。

| **事件名称** | **目的** |
| :--- | :--- |
| first_interaction | 玩家在第一次打开游戏后完成任一交互。 |
| tutorial_start | 玩家启动教程。 |
| tutorial_step | 玩家通过教程中的一个重要步骤。 |
| tutorial_complete | 玩家完成教程。 |
| tutorial_skip | 玩家跳过教程。 |

 
## 参与事件
参与事件有助于了解玩家是否参与游戏，以及他们是否执行与用户留存和游戏传播高度相关的操作。

| **事件名称** | **目的** |
| :--- | :--- |
| push_notification_enable | 玩家启用推送通知。 |
| push_notification_click | 玩家响应推送的消息。 |
| chat_msg_sent | 玩家发送聊天消息。 |
| achievement_unlocked | 玩家完成一项成就。 |
| achievement_step | 玩家在迈向目标成就的过程中完成一个重要步骤。 |
| user_signup | 玩家连接到社交网络。 |
| social_share | 玩家通过社交网络分享邀请或礼物等内容。 |
| social_accept | 玩家接受通过社交网络分享的内容。 |

## 变现事件

变现事件跟踪收入和游戏经济以帮助您平衡资源并改善收入来源。

| **事件名称** | **目的** |
| :--- | :--- |
| store_opened | 玩家打开商店。 |
| store_item_click | 玩家在商店中选择一件商品。 |
| iap_transaction | 玩家花费真实货币进行应用内购 (IAP)。 | 
| item_acquired | 玩家在游戏中获得资源。 | 
| item_spent | 玩家在游戏中消耗一件物品。 |
| ad_offer | 玩家有机会观看广告。 | 
| ad_start | 玩家开始观看广告。 |
| ad_complete | 玩家看完广告。 |
| ad_skip | 玩家在看完之前跳过广告。 |
| post_ad_action | 玩家完成广告提示的操作。 |

## 脚本标准事件

要从脚本分发标准事件，请添加以下命名空间：

    using UnityEngine.Analytics;

注意：使用 Unity Asset Store 的 Standard Events SDK 时，请改用命名空间 `UnityEngine.Analytics.Experimental`。

现在您可以使用 [AnalyticsEvent](../ScriptReference/Analytics.AnalyticsEvent.html) 类来分发标准事件。每个标准事件都有相应的静态函数。例如，要发送 game_start 事件，请调用以下函数：

```
AnalyticsEvent.GameStart();
```

许多标准事件都有必需参数，有一些事件具有可选参数。您还可以在字典中添加自己的附加参数。例如，如果想要跟踪玩家使用 ScreenVisit 事件访问了哪些屏幕，但也想跟踪他们从哪个屏幕进行访问，则可以在字典对象中传递该信息：

```
public static void ReportScreenVisit(string screenName, string currentScreenName)
{
    AnalyticsEvent.ScreenVisit(screenName, new Dictionary<string, object>
    {
        { "from_screen", currentScreenName }
    });
}
```

必需参数、可选参数（仅统计您使用的参数）和附加参数的总数不得超过十个。换言之，如果一个事件函数有 2 个必需参数，并且您设置了 2 个可选参数中的 1 个，则还可以将包含最多 7 个附加键/值对的字典传递给该函数。如果在同一事件中同时设置了 2 个可选参数，则该字典只能包含最多 6 个键/值对。

**标准事件代码示例**

以下类提供的示例使用标准 MonoBehaviour 事件函数来分发与开始和结束关卡相关的标准事件。

The [Awake](../ScriptReference/MonoBehaviour.Start.html) function dispatches the level_start event. The example provides both the level name and the level index, but only one is required. The example also uses the name and index from the [Scene](../ScriptReference/SceneManagement.Scene.html) object, but you can use different values. 

The [OnDestroy](../ScriptReference/MonoBehaviour.OnDestroy.html) function dispatches a level_complete, level_fail, level_skip, or level_quit event, based on the current level state value (which, in a real game, would be set by other code in the level using the `SetLevelPlayState()` function defined in this example). The example adds some additional parameters to these events using a dictionary called “customParams”. 

```
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.Analytics;

public class LevelEventManager : MonoBehaviour {
    public enum LevelPlayState {InProgress, Won, Lost, Skip, Quit}

    private Scene thisScene;
    private LevelPlayState state = LevelPlayState.InProgress;
    private int score = 0;
    private float secondsElapsed = 0;
    private int deaths = 0;

    void Awake () {
        thisScene = SceneManager.GetActiveScene();
        AnalyticsEvent.LevelStart(thisScene.name, 
                                      thisScene.buildIndex);
    }

    public void SetLevelPlayState(LevelPlayState newState){
        this.state = newState;
    }

    public void IncreaseScore(int points){
        score += points;
    }

    public void IncrementDeaths(){
        deaths++;
    }

    void Update(){
        secondsElapsed += Time.deltaTime;
    }

    void OnDestroy(){
        Dictionary<string, object> customParams = new Dictionary<string, object>();
        customParams.Add("seconds_played", secondsElapsed);
        customParams.Add("points", score);
        customParams.Add("deaths", deaths);

        switch(this.state){
        case LevelPlayState.Won:
            AnalyticsEvent.LevelComplete(thisScene.name,
                                             thisScene.buildIndex, 
                                             customParams);
            break;
        case LevelPlayState.Lost:
            AnalyticsEvent.LevelFail(thisScene.name,
                                         thisScene.buildIndex, 
                                         customParams);
            break;
        case LevelPlayState.Skip:
            AnalyticsEvent.LevelSkip(thisScene.name,
                                         thisScene.buildIndex, 
                                         customParams);
            break;
        case LevelPlayState.InProgress:
        case LevelPlayState.Quit:
        default:
            AnalyticsEvent.LevelQuit(thisScene.name,
                                         thisScene.buildIndex, 
                                         customParams);
            break;
        }
    }
}
```

请注意，本示例仅使用一次 `OnDestroy()` 中创建的自定义参数字典。但是，在分发频繁使用这种字典的事件时，应创建该字典的单个实例并重复使用它，而不是每次创建一个新字典。重复使用字典对象可减少未来必须进行垃圾回收的内存分配。请参阅 [Analytics 事件参数](UnityAnalyticsEventParameters.html)以了解更多信息。

---

* <span class="page-edit">2018-03-02 Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">截至 2018-03-02，服务与 Unity 5.2 之后的版本兼容，但是版本兼容性可能会发生变化</span>

* <span class="page-history">5.2 中的新功能</span>

