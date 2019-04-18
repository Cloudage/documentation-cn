# Facebook Player Settings

本页面将详细介绍 Facebook 平台特有的__播放器设置 (Player Settings)__。有关常规设置的描述，请参阅[播放器设置](class-PlayerSettings.html)和[启动画面设置](class-PlayerSettingsSplashScreen.html)。

请注意，Facebook 构建目标使用现有的 [WebGL](class-PlayerSettingsWebGL.html) 和 Windows [独立平台](class-PlayerSettingsStandalone.html)构建目标，因此这些目标的 Player Settings 也将适用。

## 发布设置 (Publishing Settings)

![](../uploads/Main/class-PlayerSettingsFacebook-0.png) 

| 属性| 功能 |
|:---|:---| 
| Facebook SDK Version| 选择项目要使用的 Facebook SDK 版本。此菜单将填充 Facebook 最新发布的与 Unity 版本兼容的 SDK 版本。 |
| AppID| 您的 Facebook AppID，由 Facebook 用于识别您的应用程序。请参阅 Facebook 关于如何设置该标识的入门介绍页面。 |
| Upload access token| 这是一个上传访问令牌，用于授权 Unity Editor 将您的应用程序内部版本上传到 Facebook。可在 Facebook 的应用程序配置页面的“Web Hosting”选项卡下获取此信息。 |
| Show Windows Player Settings| 打开独立平台播放器设置，这些设置会影响所有 Facebook Gameroom 构建。 |
| Show WebGL Player Settings| 打开 WebGL 播放器设置，这些设置会影响所有 Facebook WebGL 构建。 |
| FB.Init() Parameters| 这些参数将影响 Facebook SDK 在 facebook.com 网页上的初始化方式。记录在这里。 |

<br/> 

---

* <span class="page-edit"> 2017-05-16  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>


