如何进行立体渲染
================================

DirectX11.1 立体 3D 支持的立体渲染。
----------------------------------------------------------------

最低要求为：

* Windows 8.1。
* Unity Pro 4.5。
* 支持 Direct X 11 的显卡。
* 需要安装具备立体支持功能的显卡驱动程序，并且您需要使用双 DVI 或 DisplayPort 线缆，单 DVI 不能满足要求。

Player Settings 中的 Stereoscopic 复选框只能用于 DirectX11.1 的立体 3D 支持。目前不使用 AMD 的四缓冲区扩展。确保[此示例](https://code.msdn.microsoft.com/windowsapps/Direct3D-111-Simple-Stereo-9b2b61aa)在您的机器上有效。立体支持在全屏模式和窗口化模式下均有效。

启动游戏时，按住 Shift 可调出分辨率对话框。如果检测到支持 Stereo3D 的显示器，分辨率对话框中将显示相应的复选框。Camera 上有几个与 API 相关的选项：stereoEnabled、stereoSeparation 和 stereoConvergence。请使用这几个选项来调整效果。在场景中只需要一个摄像机，两只眼睛的渲染是由这些参数来处理的。

请注意，目前此复选框不适用于 Oculus（或目前所知的其他 VR 头盔）。
     

1.使用[此示例](https://code.msdn.microsoft.com/windowsapps/Direct3D-111-Simple-Stereo-9b2b61aa)检查您的设置。
2.在 Player Setting 中选中 Stereoscopic Rendering 复选框和 Use Direct3D 11 复选框。
3.发布为 32 位和 64 位应用程序。
4.以单摄像机和双摄像机进行尝试。
5.启动应用程序时按住 Shift 可在分辨率对话框中看到 Stereo 3D 复选框。根据项目的播放器设置，分辨率对话框可能被禁止或者始终被启用。

注意：目前，如果将 Unity 设置为在线性颜色空间中渲染，则会破坏立体渲染。这似乎是 Direct3D 的局限性。似乎还有一个问题：如果您启用了某些实时阴影（在前向渲染中），则 ``camera.stereoconvergence`` 参数完全不起作用。在延迟光照中，您将获得一些阴影，但左右眼之间不一致。
 
 
Oculus 的立体渲染
---------------------------------
 
可从 [Oculus 站点](https://developer.oculus.com/downloads/)获取面向 Oculus 的 Unity 免费集成。您可以使用 Unity 4.6 以上的版本和 Oculus 集成包将所有 VR 内容部署到 Rift。

入门：将 Unity 4 Oculus 集成包导入到 Unity 中，然后打开演示场景，便可以开始工作了。

该版本支持 Windows、Mac、Linux 和 Gear VR，可通过纯 C# 封装器来完全访问 LibOVR。

功能包括：

* 高质量的编辑器内 VR 预览
* 简单的定向到 Rift 渲染
* LibOVR/VRLib 中的镜头矫正、TimeWarp 和动态预测
* 针对驾驶舱、UI 等的分层摄像机支持
* 针对 XInput 控制器的跨平台支持
* 在所有世界比例下均提供一致的 IPD 和头部模型，确保舒适度
* 通过 DK2 延迟测试器进行动态预测
* 用于运动、跟踪器访问和 Rift 状态的实用程序

另请参阅 [Oculus Unity 集成指南 (Oculus Unity Integration Guide)](http://static.oculus.com/sdk-downloads/documents/OculusUnityIntegrationGuide_0.4.3.pdf)。

