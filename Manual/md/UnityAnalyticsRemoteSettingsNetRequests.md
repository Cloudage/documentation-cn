# Remote Settings 网络请求

在 Unity Analytics Dashboard 中[创建 Remote Settings 键/值对](UnityAnalyticsRemoteSettingsCreating.html)时，Unity Analytics 服务会将该设置存储在您为项目指定的 __Configuration__（__Release__ 或 __Development__）中。每当玩家启动应用程序的新会话时，Unity 都会提出网络请求以从 Analytics 服务获取最新配置。在玩家启动应用程序或者返回到已进入后台至少 30 分钟的应用程序时，Unity 会认为启动了新会话。在运行应用程序的常规非开发版时，Unity 会请求 __Release__ 配置，而在运行开发版时请求 __Development__ 配置。Unity Editor 中的 Play 模式被视为开发版。

**注意：**为了让 Unity 请求 __Development__ 配置，必须使用 Unity 5.6.0p4+、5.6.1p1+、2017.1+ 或 Unity 5.5.3p4+ 来构建应用程序，并在 [Build Settings 窗口](BuildSettings.html)中勾选 __Development Build__ 复选框。如果使用较低版本的 Unity 来构建游戏，则 Unity 始终请求 __Release__ 配置。

有关 Remote Settings 配置的网络请求完成时，[RemoteSettings](../ScriptReference/RemoteSettings.html) 对象会将一个 `Updated` 事件分发给任何已注册的事件处理程序，包括由 [Remote Settings 组件](UnityAnalyticsRemoteSettingsComponent.html)注册的事件处理程序。

如果计算机或设备没有互联网连接而无法与 Analytics 服务通信，Unity 将使用自己上次收到并保存的配置。在使用保存的配置时，`RemoteSettings` 对象仍然会分发 `Updated` 事件。但是，如果 Unity 尚未保存任何设置（例如玩家第一次运行游戏时没有网络连接），则 `RemoteSettings` 对象不会分发 `Updated` 事件，因此不会更新游戏变量。通过网络请求 Remote Settings 配置是一个异步过程，在初始场景完成加载之前可能无法完成，或者根本无法完成，因此您应该始终将游戏变量初始化为合理的默认值。

**注意：**Unity 下载 Remote Settings 配置时使用的 Web 服务是只读的，但未受保护。这意味着配置可能被第三方读取。您不应将敏感或机密信息放入 Remote Settings。同样，保存的设置文件可能由终端用户读取和修改（尽管在下次启动具有可用互联网连接的会话时会覆盖任何修改）。

---

* <span class="page-edit">2017-05-30 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">截至 2017-05-30，服务与 Unity 5.5 之后的版本兼容，但是版本兼容性可能会发生变化。</span>
 
* <span class="page-history">2017.1 中的新功能</span>
