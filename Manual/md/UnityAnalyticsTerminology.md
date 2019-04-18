# Analytics 指标、细分段和术语

本术语表定义了 Unity Analytics 系统和文档所用的指标和术语。

## 指标

Unity Analytics 系统会收集或计算以下指标并将其显示在 Analytics Dashboard Overview 和 Data Explorer 页面上的报告中：

### 活跃玩家指标：

|:---|:---| 
|__DAU__<br/>(Daily Active Users)| The number of different players who started a session on a given day. The Unity Analytics system anchors its days on Coordinated Universal Time (UTC), so the DAU figure counts players between 0:00 UTC and 24:00 UTC, no matter which timezone they are located in. A new session is counted when a player launches your game or brings a suspended game to the foreground after 30 minutes of inactivity.<br/><br/>DAU includes both new and returning players. |
|__DAU per MAU__<br/> (DAU/MAU) | The percentage of monthly active users who play on a given day. Also known as Sticky Factor in the analytics and game industries, this metric is often used as one estimate of player engagement.<br/><br/>Note that the metric includes both returning and new players. An influx of new players could mask the exit (churn) of existing players. In other words, a steady DAU per MAU metric doesn’t mean high player engagement if players are leaving your game just as fast as they join. Always examine retention metrics as well. |
|__MAU__<br/>(Monthly Active Users)| The number of players who started a session within the last 30 days.|
|__New Users__| Users who played your game for the first time.<br/><br/> Unity records a persistent, randomized ID value to the [PlayerPrefs](../ScriptReference/PlayerPrefs.html) and uses the id to identify returning players. Note that if the PlayerPrefs file is cleared or deleted, then the player is counted as a new user the next time they launch the game. The PlayerPrefs file is deleted when a player uninstalls and then reinstalls your game (but not when updating).  |
|__Number of Users__| The cumulative number of unique players over the last 90 days. Users who have not played in more than 90 days are removed from the count. |
|__Percentage of Population__| Your player population as a percentage. Typically only useful when combined with a segment. Calculated as the percentage of the Number of Users metric who are members of a specified segment. |

### 会话指标：
|:---|:---| 
|__Sessions per User__| The average number of sessions per person playing on a given day.<br/>Also known as Average Number of Sessions per DAU. |
|__Total Daily Playing Time__| The cumulative playing time of all people playing on a given day. |
|__Total Daily Playing Time per Active User__| The average playing time of people playing on a given day.|
|__Total Sessions Today__| The total number of sessions by all people playing on a given day. Also known as Total Sessions.|

### 留存指标：

Unity Analytics 使用称为 *N 天留存* (N-Day Retention) 的方法来计算留存度。在数学上，“N 天留存”的算法是第一次启动会话后正好玩 N 天的玩家数除以 N 天前的新用户数。

Unity Analytics 计算的标准留存指标包括：

|:---|:---| 
| __Day 1 Retention__| The percentage of players who returned to your game one day after playing the first time. |
| __Day 7 Retention__| The percentage of players who returned to your game seven days after playing the first time. |
| __Day 30 Retention__| The percentage of players who returned to your game thirty days after playing the first time. |

### 变现指标：

|:---|:---| 
|__ARPDAU__<br/>(Average Revenue Per Daily Active User)|  The average revenue per user who played on a given day.<br/><br/>Revenue includes money earned from Unity Ads, verified Unity IAP transactions, and verified IAP transactions reported using the [Analytics.Transaction()](../ScriptReference/Analytics.Analytics.Transaction.html) function.|
|__ARPPU__<br/>(Average revenue Per Paying User)|   Average verified IAP revenue per user who completed a verified IAP transaction. |
|__Number of Unverified Transactions__| The total number of IAP transactions, whether or not they have been verified.<br/><br/>IAP transactions include Unity IAP purchases and purchases reported using the [Analytics.Transaction()](../ScriptReference/Analytics.Analytics.Transaction.html) function.  |
|__Number of Verified Transactions__| IAP transactions that have been verified through the appropriate app store. IAP verification is currently supported by the Apple App Store and the Google Play Store. |
|__Total IAP Revenue__| The total IAP revenue, including revenue from both verified and unverified transactions.<br/><br/>IAP transactions include Unity IAP purchases and purchases reported using the [Analytics.Transaction()](../ScriptReference/Analytics.Analytics.Transaction.html) function.  |
|__Total Verified Revenue__| Revenue from Unity Ads and verified IAP transactions. IAP verification is currently supported by the Apple App Store and the Google Play Store. |
|__Unverified IAP Revenue__| IAP revenue from sources that do not support verification and from transactions that failed verification. Transactions can fail verification because they are fraudulent or because of missing or malformed information. For example, if you haven’t configured your Google API key on the Analytics Dashboard, attempts to verify Google Play Store transactions will fail. See [Receipt Verification](UnityAnalyticsReceiptVerification.html). |
|__Verified IAP Revenue__| Revenue from verified IAP transactions. IAP verification is currently supported by the Apple App Store and the Google Play Store. |
|__Verified Paying Users__| Players who made verified IAP purchases. IAP verification is currently supported by the Apple App Store and the Google Play Store. |

### 广告指标：

|:---|:---| 
|__Ad ARPU__<br/>(Average Revenue Per User)| Average Unity Ads revenue per player.|
|__Ad Revenue__| Total Unity Ads revenue. |
|__Ad Starts__| The number of video ads that started playing. |
|__Ads per DAU__| The number of ads started per active player on a given day. |
|__eCPM__<br/> (estimated Cost Per Mille) | The estimated revenue for 1000 ad impressions for your app. See [What is ECPM](https://support.unity3d.com/hc/en-us/articles/115000564306-What-is-eCPM-).|



有关 Ads 的更多信息，请参阅 [Unity Ads Knowledge Base](https://unityads.unity3d.com/help/index)。

## 细分段

细分段是指玩家群体的子集，按照主要差异因素（如国家/地区、平台、经验级别或消费模式）进行划分。细分段可与 __Data Explorer__、__Funnel Analyzer__ 和 __Remote Settings__ 功能结合使用。

The Analytics Service provides a set of standard segments and provides the ability to create your own segments. You can view the rules defining the standard segments and create your own custom segments on the Analytics Dashboard [Segment Builder](UnityAnalyticsSegmentBuilder.html) page. 

### 生命周期 (Life Cycle) 细分段：

生命周期细分段按照玩家首次玩游戏以来的天数将玩家分组。该服务定义了以下生命周期组：

|:---|
|1-3 Days| 
|4-7 Days|
|8-14 Days|
|15-30 days|
|31+ Days|



注意：生命周期细分段仅包含前 90 天玩游戏的玩家。换言之，__31+ Days__ 细分段包括首次玩游戏超过 30 天而不超过 90 天的玩家。

### 地理区域细分段：

地理区域细分段按国家/地区将玩家分组。使用 IP 分析等技术对玩家进行地理定位。标准地理区域细分段包括以下国家/地区：

|:---|
|Australia|
|Canada| 
|China| 
|France| 
|Germany| 
|Japan| 
|Russia|
|South Korea|
|Taiwan|
|United Kingdom|
|United States|
|Rest of World|


*Rest of World* 细分段包括未单独列出的国家/地区的所有玩家。请注意，可定义自定义细分段以便按其他的单个国家/地区将玩家分组。（但是，这些玩家仍会包括在 Rest of World 标准细分段中。）

### 变现细分段：

变现细分段按照玩家在游戏中通过应用内购 (IAP) 花费的金额将玩家分组：

|:---|:---| 
|__Whales__|Players who have spent at least $20 in their lifetime. |
|__Dolphins__|Players who have spent between $5 and $19.99.|
|__Minnows__|Players who have spent less than $5 in their lifetime.|
|__Never Monetized__|Players who have never spent real currency.|
|__All Spenders__|Players who had made any verified or unverified in-app purchases in their lifetime.|

*Whales、Dolphins、Minnows* 的比喻是一种基于以下观察结果的常见分析类比：少数用户对典型免费 (F2P) 游戏的货币"生态系统"具有不成比例的巨大影响。


### __平台细分段__

平台细分段按照玩家玩游戏的平台分组。Analytics 系统提供两个标准平台细分段：

* Android Users

* iOS Users

### 自定义细分段

通过创建自定义细分段可以获得比标准细分段更精细的细分，还可以创建细分段来合并不同细分类别的规则。

与标准细分段不同，一旦玩家成为自定义细分段的成员，该玩家就始终是该细分段的成员。

|:---|:---| 
|__Standard and Custom Events__| Segment by whether a specific custom event has been dispatched while a user is playing. You can also specify restrictions on parameter values.  |
|__Sessions__| Segment by when and how long a user played. |
|__IAP revenue__| Segment by IAP activity. |
|__Demographics__| Segment by reported demographics. |
|__Geography__| Segment by country. |
|__Application version__| Segment by application version or bundleid. |

新细分段只适用于新数据。不会使用新细分规则来重新处理现有数据。

请参阅 [Segment Builder](UnityAnalyticsSegmentBuilder.html) 以了解有关自行定义细分段的更多信息。

## 术语

Unity Analytics 服务使用以下术语：

|:---|:---| 
|__Analytics Events__| Events dispatched to the Analytics Service by instances of your applications. Analytics events contain the data that is processed and aggregated to provide insights into player behavior. |
|__Cohort__| A group of players with at least one similar characteristic. You can define and analyze different cohorts of your userbase with [segments](UnityAnalyticsSegmentBuilder.html). |
|__Core Events__| Core events are the basic events dispatched by the Unity Analytics code in your game. These events, and the analytics based on them, become available simply by turning on Unity Analytics for a project. Core events include: app running, app start, and device info.  |
|__Standard Events__| Standard events are application-specific events that you dispatch in response to important player actions or milestones. Standard events have standardized names and defined parameter lists. |
|__Custom Events__| Custom events are freeform events that you can dispatch when an appropriate standard event is not available. Custom events can have any name and up to ten parameters. Use standard events in preference to custom events where possible. |
|__Funnel__| A funnel is a linear sequence of standard or custom events that you expect a player to complete in order. Analysis of funnels can reveal those steps in the sequence where players encounter friction or problems. For example, a funnel based on level completion events would reveal whether larger than expected numbers of players stopped playing at specific levels. See [Funnels](UnityAnalyticsFunnels.html).   |
|__Heatmaps__| Heatmaps are a spatial visualization of analytics data. The heatmap visualization displays markers for special analytics events overlaid on your scene in the Unity Editor.Heatmaps requires access to [Raw Data Export](UnityAnalyticsRawDataExport.html), a pro-subscription feature. See [Heatmaps](https://bitbucket.org/Unity-Technologies/heatmaps/wiki/v2).  |
|__Remote Settings__| Remote settings are game variables that you can set remotely on your Analytics Dashboard. See [Remote Settings](UnityAnalyticsRemoteSettings.html). |
|__Aggregated Data__| As events are received by the Analytics Service, the data is processed and aggregated. The aggregated data is the basis of most of the Dashboard reports and visualizations. |
|__Raw Data__| The individual Analytics events sent by your application. Developers with Unity Pro subscriptions can download this data using [Raw Data Export](UnityAnalyticsRawDataExport.html) for analysis in external tools or other purposes.  |
|__Segment__| Segments are subsets of your player base, split apart by key differentiators. Viewing metrics and events by segment can reveal differences in-game behavior between different groups. |
|__Session__| A single play or usage period. A new session is counted when a player launches your game or brings a suspended game to the foreground after 30 minutes of inactivity.  |



以下一般术语有助于理解：

|:---|:---| 
|__Active Users__| Players who recently played your game. Unity Analytics defines an active player as someone who has played within the last 90 calendar days.  |
|__Churn__| The rate at which users are leaving your game during a specified period. Your user churn is important in estimating the lifetime value of your users. Mathematically, churn is the complement of retention (in other words: Churn + Retention = 100%). |
|__COPPA__<br/>(Children’s Online Privacy Protection Act)| COPPA is a US law that applies to apps that collect personal information and are targeted to children under the age of 14. <br/><br/>See [COPPA FAQ](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule) and [COPPA Compliance](UnityAnalyticsCOPPA.html) |
|__Conversion Rate__|The percentage of users who complete an action or sequence of actions. For example, the conversion rate from one step of a funnel to the next is the percentage of players who make it through both steps, while the conversion rate of the funnel as a whole is the percentage of players starting the funnel who complete the entire sequence. The term conversion is also used in other contexts, such as starting and then completing a purchase, or watching an ad and then clicking on a link in the ad. |
|__CTR__<br/>(Click Through Rate)| The percentage of players who click a link in an ad displayed in your game.|
|__Engagement__| Engagement is a broad measure of how players enjoy, or are otherwise invested, in your game. Impossible to measure directly, the following metrics are frequently used to estimate engagement: Retention, DAU, MAU, DAU/MAU, number of sessions, and session length.  |
|__F2P__<br/> (Free to Play) | A business model that offers users free access to a fully functional game and a significant portion of app content. Monetization strategies for these titles generally include microtransactions that allow users to access premium features and virtual goods. |
|__Fill Rate__| The rate at which ads are available when you request one. |
|__GDPR__<br/>(General Data Protection Regulation) | A European Union law regulating data privacy. See [Unity Analytics and the EU General Data Protection Regulation](UnityAnalyticsDataPrivacy.html) for more information.
|__IAP__<br/> (In-app Purchase) | Revenue from "micro-transactions" within a game.  |
|__Impressions__| The number of times ads are seen in your game. An impression is counted even if the ad is not completed. |
|__LTV__<br/> (Lifetime Value) | The estimated value of an average player over their lifetime with your application or game.|
|__Sticky Factor__| An estimate of how compelling a game is to its players. A high “sticky factor” means that players stick with an app over time. Unity Analytics calculates Sticky Factor as the percentage of monthly active users who played on a particular day (DAU/MAU), which is a commonly used definition. Note that this definition can hide the exit of existing users with an influx of new users, so it should not be relied on as an indicator of the “health” of your game in isolation from your retention metrics. |

---
* <span class="page-edit">2017-08-29  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">2018-06-04 - Removed Demographics Segments, which are no longer supported.</span>

* <span class="page-history">Unity 2017.1 中的新功能</span>

