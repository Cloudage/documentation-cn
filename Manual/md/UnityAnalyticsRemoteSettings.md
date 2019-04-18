# Remote Settings

使用 [Unity Analytics 服务](UnityAnalytics.html)的 __Remote Settings__ 功能可以直接从 Analytics Dashboard 更改游戏或应用程序中的变量。例如，如果您担心游戏中的某些关卡可能太难或太简单，可以创建控制关卡 boss 难度的设置。然后，如果 Analytics 数据显示某些关卡 boss 太强悍或太弱，您可以在不发布更新的情况下调整游戏玩法。甚至还可以使用细分段将不同设置应用于不同的玩家群体。

Remote Settings 可以帮助您：

* 根据不同类型的玩家调整游戏
* 近乎实时地调整游戏难度曲线
* 在逐渐推出新功能的同时监测影响
* 针对不同地区或其他玩家细分段来定制游戏设置
* 打开和关闭季节性功能、假期功能或其他对时间敏感的功能
* 测试您的想法和设计

对 Remote Settings 进行更改后，启动应用程序新会话的每台计算机或设备都会下载新的配置值。无需更新应用程序。您可以将 Remote Settings 连接到单个游戏对象属性，也可以在代码中直接读取键/值对并[提供您自己的逻辑](UnityAnalyticsRemoteSettingsScripting.html)来处理设置更改。

本部分介绍[创建和更改 Remote Settings](UnityAnalyticsRemoteSettingsCreating.html)以及[在 Unity 项目中使用 Remote Settings](UnityAnalyticsRemoteSettingsUsing.html)。

---

* <span class="page-edit">2017-08-28 Page amended with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">截至 2017-08-28，服务与 Unity 5.5 之后的版本兼容，但是版本兼容性可能会发生变化。</span>
 
* <span class="page-history">[2017.1](https://docs.unity3d.com/2017.1/Documentation/Manual/30_search.html?q=newin20171) 中的新功能 <span class="search-words">NewIn20171</span></span>

* <span class="page-history">2017.1 中添加了细分的 Remote Settings：为不同细分段设置不同值</span>
