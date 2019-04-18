# 高级 Unity 移动端脚本

## 设备属性

在进行移动端开发时，可访问许多特定于设备的属性。请参阅有关 [SystemInfo.deviceUniqueIdentifier](../ScriptReference/SystemInfo-deviceUniqueIdentifier.html)、[SystemInfo.deviceName](../ScriptReference/SystemInfo-deviceName.html)、[SystemInfo.deviceModel](../ScriptReference/SystemInfo-deviceModel.html) 和 [SystemInfo.operatingSystem](../ScriptReference/SystemInfo-operatingSystem.html) 的 Unity Scripting API 页面以了解更多信息。

## 反盗版检查

盗版者经常通过移除 DRM 保护再免费重新分发应用程序来进行破解。Unity 附带反盗版检查功能，用于确定应用程序在提交到 Google Play 应用商店之后是否被篡改。

要检查应用程序是否为正版（意味着未被破解），请使用 Unity Scripting API [Application.genuine](../ScriptReference/Application-genuine.html) 属性。如果此属性返回 `false`，则可通知应用程序用户他们正在使用经过破解的应用程序，也可禁用对应用程序某些功能的访问。

**注意：**应使用 [Application.genuineCheckAvailable](../ScriptReference/Application-genuineCheckAvailable.html) 以及 [Application.genuine](../ScriptReference/Application-genuine.html) 来验证应用程序完整性。访问 [Application.genuine](../ScriptReference/Application-genuine.html) 属性是一项资源密集型操作，因此请勿在帧更新期间或运行其他时间迫切的代码时调用该操作。

## 振动支持

要触发振动，请在代码中调用 [Handheld.Vibrate](../ScriptReference/Handheld.Vibrate.html)。缺少振动硬件的设备将忽略此调用。

## 活动指示器

移动操作系统具有内置活动指示器；可在慢速操作期间激活这些指示器。

请参阅[活动指示器 Unity Scripting API](../ScriptReference/Handheld.StartActivityIndicator.html) 文档以查看示例。

## 屏幕方向

为 iOS 和 Android 创建项目时，可控制用户设备的屏幕方向。检测方向变化或强制使用特定方向对于根据用户握持设备的方式来改变游戏行为非常有用。

要检索设备方向，请使用 [Screen.orientation](../ScriptReference/Screen-orientation.html) 属性。允许的方向如下：

| **方向** | **行为**  |
|:---|:---| 
| Portrait| 设备处于纵向模式，直立握持设备，主屏幕按钮位于底部。 |
| PortraitUpsideDown| 设备处于纵向模式，但是上下颠倒，直立握持设备，主屏幕按钮位于顶部。 |
| LandscapeLeft| 设备处于横向模式，直立握持设备，主屏幕按钮位于右侧。 |
| LandscapeRight| 设备处于横向模式，直立握持设备，主屏幕按钮位于左侧。 |

通过将 [Screen.orientation](../ScriptReference/Screen-orientation.html) 设置为上述选项之一或设置为 [ScreenOrientation.AutoRotation](../ScriptReference/ScreenOrientation.AutoRotation.html) 来控制屏幕方向。

使用自动旋转时，可根据具体情况禁用某些方向。请参阅有关 [Screen.autorotateToPortrait](../ScriptReference/Screen-autorotateToPortrait.html)、[Screen.autorotateToPortraitUpsideDown](../ScriptReference/Screen-autorotateToPortraitUpsideDown.html)、[Screen.autorotateToLandscapeLeft](../ScriptReference/Screen-autorotateToLandscapeLeft.html) 和 [Screen.autorotateToLandscapeRight](../ScriptReference/Screen-autorotateToLandscapeRight.html) 的脚本 API 页面以了解更多信息。

----
* <span class="page-edit">2017-05-25 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.5 版中的更新功能</span>
