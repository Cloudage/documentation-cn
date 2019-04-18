# Unity Analytics 和欧盟一般数据保护条例 (GDPR)

一般数据保护条例 (General Data Protection Regulation, GDPR) 是欧盟关于规范欧盟公民数据隐私的法律。([https://www.eugdpr.org](https://www.eugdpr.org))。

有关 Unity 和 GDPR 的一般信息，请参阅 [https://unity3d.com/legal/gdpr](https://unity3d.com/legal/gdpr)。

有关 Unity 自身的隐私政策，请参阅 [https://unity3d.com/legal/privacy-policy.](https://unity3d.com/legal/privacy-policy)

Maintaining compliance with GDPR when you use Unity Analytics is a shared responsibility. Unity collects data to help you improve the player experience with ads and game play. Some of that data includes personally identifiable information (PII) regulated under GDPR. Unity provides tools a player can use to opt-out of the PII collection and to manage the personal data collected by Unity as required by the GDPR. Your responsibilities include showing an opt-out button and providing a link to Unity’s privacy policy from your own privacy policy. 

如果您使用 Unity Ads，Unity 会在首次在手机上展示广告时向玩家显示通知，让他们可以选择加入或退出涉及个人身份的信息收集。后续广告也会显示一个按钮，用户可以使用该按钮来管理其数据隐私选项。有关 GDPR 和 Unity Ads SDK 的更多信息，请参阅 [Unity Ads Knowledge Base：GDPR 合规性 (GDPR Compliance)](https://unityads.unity3d.com/help/legal/gdpr)。

如果您同时使用 Unity Ads 和 Analytics，则 Unity Ads 提供的选择退出机制适用于这两种服务。

如果您不使用 Unity Ads，但使用其他 Unity 服务，例如 Unity Analytics、IAP、Multiplayer 或 Performance Reporting，则必须使用 Unity Analytics Data Privacy 插件为玩家提供选择退出选项。该插件提供了一个可添加到游戏中的按钮，此按钮可打开 Unity 网页，让玩家可在其中管理其隐私设置。玩家根据每个设备管理每个游戏的偏好设置。Unity Analytics 不会跟踪同一玩家是否在玩使用 Unity 制作的多个游戏或者在多个设备上玩同一个游戏。

以上选项涵盖 Unity 收集的用于定制广告和玩家服务的所有数据。但是，如果您自行收集涉及个人身份信息的数据，您有责任保护和管理该数据。

最佳实践：

* 自行征求法律意见。本文档中的任何内容均不应视为法律意见。

* 阅读并理解[数据处理附录 (Data Processing Addendum, DPA)](https://unity3d.com/profiles/unity3d/themes/unity/images/pages/gdpr/Controller-Controller-DPA-5-2-2018.pdf) ([https://unity3d.com/profiles/unity3d/themes/unity/images/pages/gdpr/Controller-Controller-DPA-5-2-2018.pdf](https://unity3d.com/profiles/unity3d/themes/unity/images/pages/gdpr/Controller-Controller-DPA-5-2-2018.pdf))

* 在您的隐私政策中将 Unity 列为收集数据的第三方，并在您的隐私政策中包含 Unity 隐私政策 ([https://unity3d.com/legal/privacy-policy](https://unity3d.com/legal/privacy-policy)) 的链接。

* 请勿在标准或自定义事件中向 Unity Analytics 发送涉及个人身份的信息。

* Do not use the `Analytics.SetUserGender()` or `Analytics.SetUserBirthYear()` to send gender and age information to Unity Analytics. These APIs are deprecated.

## 使用 Unity Analytics Data Privacy 插件

可从 [Unity Asset Store](https://www.assetstore.unity3d.com/#!/content/118922) 获取 Unity Analytics Data Privacy 插件。该插件适用于以下 Unity 版本：

* 4.7
* 5.1+
* 2017.1+
* 2018.1+

该插件不支持以下平台：Linux、Windows Phone、Unity 5.5 之前的通用 Windows 平台 (UWP)、Tizen、Apple TV、Blackberry。Unity Analytics 服务现在会自动删除从这些平台上运行的游戏发送的涉及个人身份的信息。如有任何问题，请联系 [DPO@unity3d.com](mailto:DPO@unity3d.com)。

**注意：**如果您的游戏显示来自 Unity Ads 网络的广告，Unity Ads SDK 已经处理了向玩家显示数据收集选择退出选项的问题，并已根据玩家的数据隐私选项配置 Unity Analytics。如果不使用 Unity Ads 服务，您只需要使用 Unity Analytics Data Privacy 插件。

### 在 Unity 中配置 Analytics 数据隐私

如果您不使用 Unity Ads，则可以集成 Unity Analytics Data Privacy 插件，该插件可让您的玩家控制 Unity Analytics 数据收集。

要集成该插件，请执行以下操作：

1.将 Unity Analytics 数据隐私资源包导入项目。

2.使用按钮或其他 UI 控件为玩家提供选择退出选项。

The Unity Analytics Data Privacy plugin contains code that automatically queries the Unity Analytics Service to determine the opt-out status of the current player. The plugin then configures Unity Analytics based on the player’s preferences. The plugin also includes a Unity UI button prefab. This button opens a player’s personal data privacy page where they can opt-out of Unity’s data collection and view the data that Unity has collected in the past. 

**重要信息：**如果玩家启用了浏览器弹出窗口阻止程序，则其浏览器可能阻止打开数据隐私页面。有些浏览器会提示某个页面已被阻止，但其他浏览器根本没有提供任何提示。因此，应考虑在您的用户界面中添加一条消息，告知玩家弹出窗口阻止程序可能会阻止打开页面。

#### 将 Unity Analytics Data Privacy 插件导入项目

要将该插件导入项目，请执行以下操作：

1.访问 Unity Asset Store 上的 [Unity Analytics Data Privacy 插件 (Unity Analytics Data Privacy plugin)](https://www.assetstore.unity3d.com/#!/content/118922) 页面。

2.单击该页面上的 __Add to My Assets__ 按钮。

3. In your Unity project, open the __Asset Store__ window (menu: __Window__ &gt; __Asset Store__).

4.单击窗口顶部的 __Downloads__ 图标。

5.选择 __My Purchased__。

6.单击 Unity Analytics Data Opt-Out Plugin 的 __Download__ 按钮。

7.在 __Import Unity Package__ 窗口上，单击 __Import__。

#### 为玩家提供选择退出选项

The plugin includes a Unity UI button prefab. When a player clicks this button, it opens the Player Data Privacy page in a web browser. You can also provide your own user interface and open the Data Privacy page manually.

**Method 1: Using Unity UI**

1.将资源包导入项目。

2.如果使用 Unity 5.1 或更早版本来构建项目，必须调用 `DataPrivacy.Initialize()`。（更高版本的 Unity 上会自动调用 `Initialize()`。）

3. If you do not already have a [Canvas](UICanvas.html) object in the scene, add one. (An [EventSystem](script-EventSystem.html) is also required and should be added automatically when you add the **Canvas**.)

4. Drag the  **DataPrivacyButton** prefab to the __Canvas__ object in the scene. Find the prefab in the Project window, in the **DataPrivacy/Runtime** folder.

5.根据需要调整按钮的位置、图形和文本。

6.该按钮已连接到 Data Privacy API，因此单击该按钮时会打开玩家的个人数据管理页面。此页面将在 Web 浏览器中打开。

**Method 2: Using you own UI**

要使用您自己的用户界面创建该按钮，您可以请求用户数据选择退出页面的 URL，然后在浏览器或 Web 视图中打开该 URL：

1.将资源包导入项目。

2.创建您自己的 UI 控件，告知玩家可以选择退出数据收集。

    **Note:** The Data Privacy plug-in includes an icon in the **DataPrivacy/Runtime** folder. Unity encourages you to use this icon on your data privacy button (or similar control) to provide a consistent graphical cue to players encountering data privacy controls in Unity games.

3.如果使用 Unity 5.1 或更早版本来构建项目，必须调用 `DataPrivacy.Initialize()`。（更高版本的 Unity 上会自动调用 `Initialize()`。）

4.为了响应玩家对此控件的单击操作或与此控件的交互，调用 `DataPrivacy.FetchPrivacyUrl()` 函数。此函数采用在网络请求完成时调用的 `Action<string>` 对象。还可以选择传入第二个 `Action<string>` 函数来处理故障情况。

5.在 `FetchPrivacyUrl()` 请求的处理程序中，打开在浏览器中收到的 URL。为此，可使用 `Application.OpenURL()`。

例如，以下脚本将打开 Player Data Privacy 页面来响应对游戏对象的单击：

    using System;
    using UnityEngine;
    using UnityEngine.Analytics;

    public class OptOutHandler : MonoBehaviour {

        static void OnFailure(string reason)
        {
            Debug.LogWarning(String.Format("Failed to get data privacy page URL: {0}", reason));
        }

        void OnURLReceived(string url)
        {
            Application.OpenURL(url);
        }

        public void OpenDataURL()
        {
            DataPrivacy.FetchPrivacyUrl(OnURLReceived, OnFailure);
        }

        void OnMouseOver(){
            if(Input.GetMouseButtonDown(0)){
                OpenDataURL();
            }
        }
    }

#### 将 Data Privacy 插件与旧版 Unity 和 Unity Analytics 结合使用

在 Unity 5.2 之前，Unity 通过 Unity Asset Store 资源包提供 Analytics 支持。您可以将 Data Privacy 插件与这些旧版本的 Unity Analytics 一起使用，但在调用其他任何函数（以及在玩家有机会单击 Data Privacy 按钮之前），必须先初始化隐私插件。初始化该插件的最佳位置是在调用 `UnityAnalytics.StartSDK()` 来初始化 Analytics 插件之后：

```
    using UnityEngine;
        using UnityEngine.Cloud.Analytics;
        using UnityEngine.Analytics;

        public class UnityAnalyticsIntegration : MonoBehaviour {

            void Start () {
                const string projectId = "SAMPLE-UNITY-PROJECT-ID";
                UnityAnalytics.StartSDK (projectId);
                DataPrivacy.Initialize ();
            }
        }
```

注意：Unity 5.2 或更高版本会自动调用 `DataPrivacy.Initialize()`。除非使用 Asset Store 资源包来实现 Analytics 支持，否则您无需自己调用该函数。

### Unity Analytics Data Privacy API

#### DataPrivacy 类

DataPrivacy 类根据玩家的数据隐私管理选项来配置 Unity Analytics 服务。

命名空间：UnityEngine.Analytics

```
    public class DataPrivacy 
```

DataPrivacy 类自动获取玩家的数据隐私状态，并相应配置 Analytics 服务。

使用 `FetchPrivacyUrl()` 函数来获取玩家个人数据管理页面的 URL。打开该 URL 即可为玩家提供管理其数据隐私设置的选项。

---

##### Initialize()

准备 Data Privacy API 以供使用。

声明

```
    public static void Initialize() 
```

备注

此函数会创建一个隐藏的游戏对象，并会将 DataPrivacy 类的实例作为组件添加到该游戏对象。

在 Unity 5.1 或更早版本中，应在应用程序启动时尽早调用 `Initialize()`（理想情况下是在调用 `UnityAnalytics.StartSDK (projectId)` 之后立即进行）。更高版本的 Unity 会自动调用 `Initialize()`。

---

##### FetchOptOutStatus(Action&lt;bool&gt;)

获取玩家的当前选择退出状态并配置 Unity
Analytics 服务。

声明

```
    public static void FetchOptOutStatus(Action<bool> optOutAction = null) 
```

参数

* [optional] `Action<bool> optOutAction` — 当 Unity 已获取玩家的选择退出状态时调用的 Action 对象。如果玩家已选择退出个人数据收集，则传递给 Action 的布尔参数为 `true`，否则为 `false`。

备注

该函数根据玩家选择的数据收集选项来相应配置 Analytics 服务。该状态将缓存起来以供玩家的计算机或设备脱机时使用。

游戏启动时会自动调用 `FetchOptOutStatus()`。在打开玩家的数据隐私管理 URL 后，玩家返回游戏时，也应该调用此函数，从而立即应用他们在该页面上所做的任何更改。

您还可以使用 `FetchOptOutStatus()` 通过传递 `Action<bool>` 函数参数来获得玩家的选择退出选项。该插件会在网络获取请求完成时调用提供的 `optOutAction` 函数。如果玩家已选择退出，则传递给 Action 的布尔参数为 `true`，否则为 `false`。如果玩家状态网络请求失败，则会使用缓存的值来调用 `optOutAction` 函数（如果从未设置过状态，则函数返回 `false`）。

```
    void OnOptOutStatus(bool optOut)
        {
            if(optOut)
            {
                Debug.Log("Player has opted-out of personal data collection.");
            }
        }

        // ...
        DataPrivacy.FetchOptOutStatus(OnOptOutStatus);
```

---

##### FetchPrivacyUrl(Action&lt;String&gt;, Action&lt;String&gt;)

获取玩家个人数据管理页面的 URL。

声明

```
    public static void FetchPrivacyUrl(Action<string> success, Action<string> failure = null) 
```

参数

* `Action<String> success` — 成功检索到该 URL 时要调用的 Action 对象。传递给 Action 的字符串包含该 URL。

* [optional] `Action<String> failure` — Unity 无法检索该 URL 时要调用的 Action 对象。传递给 Action 的字符串包含失败原因。

备注

应在浏览器或 Web 视图中打开传递给 success 函数的 URL，让玩家有机会管理他们的数据保护选项。您可以使用 `Application.OpenURL()` 打开该页面。

该 URL 在短时间内有效。务必在打开 URL 之前即时获取 URL。

应在玩家返回游戏后立即调用 `FetchOptOutStatus()` 来应用所有更改。否则，对数据收集选项的所有更改都将在下次玩家启动游戏时应用。



---
* <span class="page-edit">2018-05-18  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
* <span class="page-history">Removed Editor menu command for inserting the Data Privacy button. Added the Data Privacy Button prefab.</span>
* <span class="page-history">Unity 2018.1 中的新功能</span>
