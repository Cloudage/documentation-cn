GUI 层 (GUI Layer)（旧版）
=========

**请注意：***该组件涉及将 UI 纹理和图像绘制到屏幕的传统方法。您应改用 Unity 的最新 [UI 系统](UISystem.html)。此外，这与 [IMGUI 系统](GUIScriptingGuide.html)无关。*

![](../uploads/Main/Inspector-GUILayer.png) 

通过将 __GUI 层 (GUI Layer)__ 组件附加到摄像机，可启用 2D GUI 渲染。

当 GUI 层附加到摄像机时，它将渲染场景中的所有 [GUI 纹理](class-GUITexture.html)和 [GUI 文本](class-GUIText.html)。GUI 层不会对 [UnityGUI](GUIScriptingGuide.html) 有任何影响。

通过单击 __Inspector__ 中的 GUI Layer 复选框，可在单个摄像机中启用和禁用 GUI 渲染。
