AppCallbacks 类
==================


此类可称为主应用程序和 Unity 引擎之间的桥梁。在本文中，我们将尝试解释每次调用 AppCallbacks 时的具体行为。让我们构建解决方案并探索 App.xaml.cs 文件（如果使用 .NET 脚本后端，则会创建该文件）。

````
sealed partial class App : Application
{
	private WinRTBridge.WinRTBridge _bridge;
	private AppCallbacks appCallbacks;
	public App()
	{
		this.InitializeComponent();
		appCallbacks = new AppCallbacks(false);
	}

	protected override void OnLaunched(LaunchActivatedEventArgs args)
	{
		Frame rootFrame = Window.Current.Content as Frame;
		if (rootFrame == null)
		{
			var mainPage = new MainPage();
			Window.Current.Content = mainPage;
			Window.Current.Activate();

			_bridge = new WinRTBridge.WinRTBridge();
			appCallbacks.SetBridge(_bridge);

			appCallbacks.SetSwapChainBackgroundPanel(mainPage.GetSwapChainBackgroundPanel());

			appCallbacks.SetCoreWindowEvents(Window.Current.CoreWindow);

			appCallbacks.InitializeD3DXAML();
		}

		Window.Current.Activate();
	}
}
````

**private WinRTBridge.WinRTBridge \_bridge;**

首先，WinRTBridge 由 Unity 在内部用于执行一些本机到托管和托管到本机的操作，并不适合开发人员使用。由于某些 WinRT 平台限制，无法从 Unity 引擎代码创建 WinRTBridge，因此需要在这里创建 WinRTBridge 并通过 appCallbacks.SetBridge(_bridge) 传递给 Unity 引擎。

**appCallbacks = new AppCallbacks(false);**

现在，让我们进一步了解 AppCallbacks 类。创建该类时，即指定游戏将在其他不同的线程上运行，出于向后兼容性的原因，也可指定应用程序可在 UI 线程上运行，但不建议这样做，因为 Microsoft 有限制 - 如果应用程序在 5 秒后没有响应，您将无法通过 WACK（Windows 应用程序认证），请在此处阅读更多内容 - [http://msdn.microsoft.com/en-us/library/windowsphone/develop/hh184840(v=vs.105).aspx](http://msdn.microsoft.com/en-us/library/windowsphone/develop/hh184840(v=vs.105).aspx)，想象一下，如果第一个关卡很大，可能需要花费大量的加载时间，因为应用程序在 UI 线程上运行，在关卡完全加载之前，UI 将无响应。这就是为什么建议始终在其他不同的线程上运行游戏的原因。

在此处阅读有关 UI 线程的更多信息 - [http://msdn.microsoft.com/en-us/library/windows/apps/hh994635.aspx](http://msdn.microsoft.com/en-us/library/windows/apps/hh994635.aspx)

**注意：**位于 App.xaml.cs 和 MainPage.xaml.cs 中的代码始终在 UI 线程上运行（除非从 InvokeOnAppThread 函数进行调用）。

**appCallbacks.SetSwapChainBackgroundPanel(mainPage.GetSwapChainBackgroundPanel());**

此语句简单地将 XAML 控件传递给 Unity，此控件将用作 DirectX 11 的渲染目标。

**appCallbacks.SetCoreWindowEvents(Window.Current.CoreWindow);**

设置 Unity 的核心窗口，Unity 订阅以下事件（可能会有更多，具体取决于更新此信息的时间）：

* VisibilityChanged
* Closed
* PointerCursor
* SizeChanged
* Activated
* CharacterReceived
* PointerPressed
* PointerReleased
* PointerMoved
* PointerCaptureLost
* PointerWheelChanged
* AcceleratorKeyActivated

**appCallbacks.InitializeD3DXAML();**

这是 Unity 的主要初始化函数，负责执行以下操作：

* 解析 AppCallbacks.AddCommandLineArg() 设置的命令行参数
* 初始化 DirectX 11 设备
* 加载第一个关卡

此时，当 Unity 完成加载第一个关卡的操作时，它将进入主循环。

其他函数
-----

* **void InvokeOnAppThread(AppCallbackItem item, bool waitUntilDone)**

  在应用程序线程上调用委托，当您想从 UI 线程调用脚本函数时非常有用。

* **void InvokeOnUIThread(AppCallbackItem item, bool waitUntilDone)**

  在 UI 线程上调用委托，当您想从脚本调用特定于 XAML 的 API 时非常有用。

* **bool RunningOnAppThread()**
  
  如果当前在应用程序线程中运行，返回 true。

* **bool RunningOnUIThread()**
  
  如果当前在 UI 线程中运行，返回 true。

* **void InitializeD3DWindow()**
  
  D3D 应用程序的初始化函数。

* **void Run()**
  
  D3D 应用程序使用的函数，用于进入主循环。

* **bool IsInitialized()**

  第一个关卡完全加载时，返回 true。

* **void AddCommandLineArg(string arg)**

  为应用程序设置命令行参数，必须在 InitializeD3DWindow 和 InitializeD3DXAML 之前调用。

* **void SetAppArguments(string arg)** / **string GetAppArguments()**

  设置应用程序参数，稍后可从 Unity API 访问 - **UnityEngine.WSA.Application.arguments**。

* **void LoadGfxNativePlugin(string pluginFileName)**

  此函数已过时，不执行任何操作。在以前的 Unity 版本中，需要使用此函数才能为回调（如 UnityRenderEvent）注册原生插件。所有插件现在都自动注册。未来更新中将删除此函数。


* **void ParseCommandLineArgsFromFiles(string fileName)**
  
  解析文件中的命令行参数，参数必须用空格分隔。

* **bool UnityPause(int pause)**

  如果传递 1，则暂停 Unity，如果传递 0，则取消暂停，希望暂时冻结游戏时（比如创建游戏的快照时）有用。

* **void UnitySetInput(bool enabled)**
  
  启用/禁用输入。

* **bool UnityGetInput()**
  
  如果 Unity 将处理传入的输入，返回 true。

* **void SetKeyboardTriggerControl(Windows.UI.Xaml.Controls.Control ctrl)**

  设置用于触发屏幕键盘的控件。在脚本中请求屏幕键盘时，此控件将直接获得焦点。应该用控件进行调用，这样会打开处于焦点状态的键盘。

* **Windows.UI.Xaml.Controls.Control GetKeyboardTriggerControl()**

  返回控件，即当前用于触发键盘输入的控件。请参阅 SetKeyboardTriggerControl。

* **void SetCursor(Windows.UI.Core.CoreCursor cursor)**

  设置系统光标。为 CoreWindow 和独立输入源（如果使用）设置给定光标。

* **void SetCustomCursor(unsigned int id)**

  将系统光标设置为自定义设置。参数为光标资源 ID。为 CoreWindow 和独立输入源（如果使用）设置光标。
