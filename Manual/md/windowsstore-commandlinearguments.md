#通用 Windows 平台：命令行参数

通用 Windows 应用程序默认情况下不接受命令行参数，因此要传递命令行参数，必须从 App.xaml.cs/cpp 或 App.cs/cpp 调用一个特殊函数。例如 **appCallbacks.AddCommandLineArg("-nolog");**
必须在 **appCallbacks.Initialize()** 函数之前调用该函数。

## 参数

* **-nolog** - 不生成 UnityPlayer.log。

* **-force-driver-type-warp** - 强制为 DirectX 11.0 WARP 设备（更多信息：[http://msdn.microsoft.com/en-us/library/gg615082.aspx](http://msdn.microsoft.com/en-us/library/gg615082.aspx)）

* **-force-gfx-direct** - 强制为单线程渲染。

* **-force-d3d11-no-singlethreaded** - 强制在没有 D3D11_CREATE_DEVICE_SINGLETHREADED 标志的情况下创建 DirectX 11.0。

* **-dontConnectAcceleratorEvent** - 禁止连接到 `AcceleratorKeyEvent`。如果在 XAML 元素中输入有问题，这可能会有所帮助。缺点是 Unity 无法处理某些键（例如，键盘键 __F10__、__Ctrl__、__Alt__ 和 __Tab__ 可能在 Unity 中出现问题）。

* **-forceTextBoxBasedKeyboard** - 对 `TouchScreenKeyboard` 使用基于文本框的实现。仅对 UWP XAML 应用程序有影响。允许切换到不同的实现，以防默认实现出现问题。

## DirectX 功能级别

有关 DirectX 功能级别的更多信息，请参阅 [http://msdn.microsoft.com/en-us/library/windows/desktop/ff476876(v=vs.85).aspx](http://msdn.microsoft.com/en-us/library/windows/desktop/ff476876(v=vs.85).aspx)

* **-force-feature-level-9-1** - 强制为 DirectX 11.0 功能级别 9.1。

* **-force-feature-level-9-3** - 强制为 DirectX 11.0 功能级别 9.3。

* **-force-feature-level-10-0** - 强制为 DirectX 11.0 功能级别 10.0。

* **-force-feature-level-10-1** - 强制为 DirectX 11.0 功能级别 10.1。

* **-force-feature-level-11-0** - 强制为 DirectX 11.0 功能级别 11.0。

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
