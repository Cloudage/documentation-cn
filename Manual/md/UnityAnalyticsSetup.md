# 设置 Analytics
[设置项目启用 Unity 服务](https://docs.unity3d.com/Manual/SettingUpProjectServices.html)之后，即可启用 Analytics 服务。

为此，在 Services 窗口中，选择 __Analytics__，然后单击 __OFF__ 按钮以将其切换为 __ON__。如果尚未填写项目的 __Age Designation__ 必填字段，则在此阶段需要执行此操作。（同样，您可能已经为其他 Unity 服务（例如 Ads）完成了此操作）。此处指定的年龄将显示在 Services 窗口中。

 
![](../uploads/Main/AnalyticsSetup1.png) 

## 在项目中启用 Analytics

随后，必须在项目中点击 __Play__ 以验证与 Unity Analytics 的连接情况。
Unity Editor 将充当测试环境来验证 Analytics 集成。启用 Analytics 的状态下，在按 __Play__ 按钮时，Editor 会将数据（“App Start”事件）发送到 Analytics 服务。这意味着您可以直接测试 Analytics，而不必编译和发布游戏。
一旦按下 Play 按钮，就可以通过转到项目的 Analytics Dashboard 来检查该项目是否已通过验证。要访问此控制面板，请在 Services 窗口中单击 Services > Analytics > Go To Dashboard。


![](../uploads/Main/AnalyticsSetup2.png) 

 
Services 窗口的 Analytics 部分中的 __Go to Dashboard__ 按钮将在 Web 浏览器中打开控制面板。此控制面板包含用于 Analytics 数据的可视化、分析和操作工具。请参阅 [Analytics Dashboard](UnityAnalyticsDashboard.html)。

---
* <span class="page-edit">2017-08-29  Page published with [editorial review](DocumentationEditorialReview.html)
</span>
* <span class="page-history">Unity 2017.1 中的新功能</span>


