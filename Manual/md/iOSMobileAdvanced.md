高级 Unity 移动端脚本
===============================


Device Properties
-----------------

There are a number of device-specific properties that you can access. See the script reference pages for [SystemInfo.deviceUniqueIdentifier](../ScriptReference/SystemInfo-deviceUniqueIdentifier.html), [SystemInfo.deviceName](../ScriptReference/SystemInfo-deviceName.html), [SystemInfo.deviceModel](../ScriptReference/SystemInfo-deviceModel.html) and [SystemInfo.operatingSystem](../ScriptReference/SystemInfo-operatingSystem.html).


Anti-Piracy Check
-----------------

Pirates will often hack an application (by removing AppStore DRM protection) and then redistribute it for free. Unity comes with an anti-piracy check which allows you to determine if your application was altered **after** it was submitted to the AppStore.

You can check if your application is genuine (not hacked) with the [Application.genuine](../ScriptReference/Application-genuine.html) property. If this property returns __false__ then you might notify user that they are using a hacked application or maybe disable access to some functions of your application.

**Note:** [Application.genuineCheckAvailable](../ScriptReference/Application-genuineCheckAvailable.html) should be used along with __Application.genuine__ to verify that application integrity can actually be confirmed. Accessing the [Application.genuine](../ScriptReference/Application-genuine.html) property is a fairly expensive operation and so you shouldn't do it during frame updates or other time-critical code.

Vibration Support
-----------------

You can trigger a vibration by calling [Handheld.Vibrate](../ScriptReference/Handheld.Vibrate.html). However, devices lacking vibration hardware will just ignore this call.

Activity Indicator
------------------

Mobile OSes have built-in activity indicators, that you can use during slow operations. Please check [Handheld.StartActivityIndicator docs](../ScriptReference/Handheld.StartActivityIndicator.html) for sample usage.

Screen Orientation
------------------

Unity iOS/Android allows you to control current screen orientation. Detecting a change in orientation or forcing some specific orientation can be useful if you want to create game behaviors depending on how the user is holding the device.

You can retrieve device orientation by accessing the [Screen.orientation](../ScriptReference/Screen-orientation.html) property. Orientation can be one of the following:

| | |
|:---|:---|
|__Portrait__ |设备处于纵向模式，直立握持设备，主屏幕按钮位于底部。|
|__PortraitUpsideDown__ |The device is in portrait mode but upside down, with the device held upright and the home button at the top.|
|__LandscapeLeft__ |设备处于横向模式，直立握持设备，主屏幕按钮位于右侧。|
|__LandscapeRight__ |设备处于横向模式，直立握持设备，主屏幕按钮位于左侧。|

You can control screen orientation by setting [Screen.orientation](../ScriptReference/Screen-orientation.html) to one of those, or to [ScreenOrientation.AutoRotation](../ScriptReference/ScreenOrientation.AutoRotation.html).
When you enable auto-rotation, you can still disable some orientation on a case by case basis. See the script reference pages for [Screen.autorotateToPortrait](../ScriptReference/Screen-autorotateToPortrait.html), [Screen.autorotateToPortraitUpsideDown](../ScriptReference/Screen-autorotateToPortraitUpsideDown.html), [Screen.autorotateToLandscapeLeft](../ScriptReference/Screen-autorotateToLandscapeLeft.html) and [Screen.autorotateToLandscapeRight](../ScriptReference/Screen-autorotateToLandscapeRight.html)

---
* <span class="page-edit">2018-06-14 Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
 

高级 iOS 脚本
======================

确定设备世代
-----------------------------

不同的设备世代支持不同的功能，并且性能差异很大。因此，应查询设备的世代信息，并确定应禁用哪些功能来适应较慢的设备。可从 [iOS.DeviceGeneration](../ScriptReference/iOS.DeviceGeneration.html) 属性中查找设备世代信息。

有关不同设备世代、性能和受支持功能的更多信息，请参阅我们的 [iPhone 硬件指南](iphone-Hardware.html)。
