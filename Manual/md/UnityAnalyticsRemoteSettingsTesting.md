# 测试 Remote Settings

虽然您可以在 Editor 中查看两种配置的键和值，但是 Unity 在运行时只加载一种配置。Unity 在 Play 模式中以及在 [Build Settings 窗口](BuildSettings.html)内选中 __Development Build__ 复选框的情况下创建的任何版本中会使用 Development 配置。Unity 在非开发版中使用 __Release__ 配置。

在测试 Remote Settings 之前，必须首先[创建设置](UnityAnalyticsRemoteSettingsCreating.html)，然后使用 [Remote Settings 组件](UnityAnalyticsRemoteSettingsComponent.html) 或[编写所需代码](UnityAnalyticsRemoteSettingsScripting.html)将设置映射到游戏或应用程序中的变量。

要使用 __Development__ 配置进行测试，请单击 Editor 中的 __Play__ 按钮，或：

1.转至 __File__ > __Build Settings__ 以打开 Build Settings 窗口。

2.选中 __Development Build__ 复选框。

3.单击 __Build And Run__ 按钮。

要使用 __Release__ 配置进行测试：

4.转至 __File__ > __Build Settings__ 以打开 Build Settings 窗口。

5.取消选中 __Development Build__ 复选框。

6.单击 __Build And Run__ 按钮。

---

* <span class="page-edit">2017-05-30 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">截至 2017-05-30，服务与 Unity 5.5 之后的版本兼容，但是版本兼容性可能会发生变化。</span>
 
* <span class="page-history">2017.1 中的新功能</span>
