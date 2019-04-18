# 构建适用于 Apple TV 的游戏

本手册页的主要目的是帮助开发者从 iOS 过渡到 tvOS。Apple TV 平台（也称为 tvOS）建立在 iOS 平台的基础上，为游戏开发者带来了新的模式和挑战。只需点击一下即可在 tvOS 上部署现有的移动端游戏，但游戏内容通常需要适应 Unity 的新输入控制方式以及游戏在大屏幕上显示的这一事实。

## 先决条件
要进行 tvOS 开发，必须满足以下先决条件：

* 第 4 代 Apple TV 设备（还需要 USB C &lt;-&gt; USB 3.0 线缆，此部件不包含在消费者套件中）。
* Xcode 7.1 或更高版本。
* 需要使用与 iOS 设备相同的方式为此设备设置资源调配。建议使用 Xcode 创建一个空的 tvOS 应用程序，由此测试该资源调配是否正常工作。


## 开始前的注意事项

* 许多 iOS 插件与 Apple TV 不兼容，因为该设备只支持 iOS 框架的一个子集。建议创建一个单独的游戏分支或副本，并在此基础上移植到 Apple TV。请联系插件提供商，要求他们更新不兼容的插件。
* 如果游戏的磁盘使用量超过 200 MB，应将游戏分成更小的部分并使用__按需加载资源 (On Demand Resources)__。如需了解 Unity 中的按需加载资源支持情况，请参阅下面的[_按需加载资源_](#OnDemandResources)部分。请注意，Bitcode 会包含在 tvOS 构建中，使可执行文件的大小增加约 130 MB。这一增加的大小不计入分发大小，因为 App Store 服务器会将其剥离。要估计 Bitcode 大小，可使用 `otool -l` 从命令行分析可执行文件中的 LLVM 部分。

## 实现输入
Apple TV 遥控器（Siri 遥控器）可用作多功能输入设备，既可用作传统菜单导航控制器、游戏控制器、陀螺仪和加速度传感器，也可用作触摸手势设备。Apple TV 遥控器输入由 Unity 进行最低限度的处理，而主要路由到相应的 Unity API。

通常，每个游戏都需要对其输入方案进行微调，以利用独特的 Apple TV 遥控器输入功能。一些游戏将受益于将该遥控器用作传统游戏控制器（具有一个模拟轴和一个额外的动作按钮），而其他游戏将受益于使用加速度计（例如，用于转向目的）。建议在将游戏移植到 tvOS 时尝试各种方案。

以下是关于访问特定 TV 遥控器功能的一些技术细节：

* 如果尚未给游戏添加 Made For iOS (MFi) 游戏控制器支持，请参阅专用的 [MFi 游戏控制器](iphone-joystick.html)页面。在 Unity Editor 中设置自定义操作映射 (Edit &gt; Project Settings &gt; Input**) 时，请使用此页面列出的映射。
* Apple TV 遥控器触摸区域同时映射到 `Input.touches`（`Touch.type` 设置为 `Indirect` 并被 Unity GUI 忽略）和常规游戏杆输入 API（例如 `Input.GetAxis("Horizontal");`）
* Apple TV 遥控器加速度和陀螺仪相应地映射到 `Input.acceleration` 和 `Input.gyro`。`Input.acceleration` 派生自内部陀螺仪 API，可能有一定的不稳定性。遗憾的是，tvOS SDK 中没有专用的加速度计 API。`Input.gyro.attitude` 派生自重力矢量，因此缺乏围绕平行于重力矢量的轴的旋转。`Input.gyro.rotationRate` 也是同样的情况。
* 遥控器上的 Pause/Play 按钮映射到按钮“X”（后者再映射到[游戏杆按钮 15](iphone-joystick.html)）。
* Apple TV 遥控器触摸区域点击映射到按钮“A”（后者再映射到[游戏杆按钮 14](iphone-joystick.html)）。
* Menu 按钮在此设备上具有特殊行为；长按调用 tvOS 任务切换器。无法覆盖此行为。短按有两种处理方式：
    * **a)** 返回 tvOS 系统主屏幕（如果 `UnityEngine.Apple.TV.Remote.allowExitToHome` 为 **true**）
    * **b)** 当 `UnityEngine.Apple.TV.Remote.allowExitToHome` 为 **false** 时，让应用程序响应点击（映射到按钮“Pause”/[游戏杆按钮 0](iphone-joystick.html)）。这是默认行为。
    * 应用程序应根据游戏的当前状态在 **a)** 和 **b)** 之间切换。如果用户正在与顶层菜单交互，则启用行为 **a)**；如果他们正在与游戏实时交互，应启用行为 **b)**，并在按下此按钮时调入游戏内暂停菜单。
* 当您轻扫至 Apple TV 遥控器的边缘时，遥控器还会生成方向键盘上/下/左/右按钮按压。请参阅 [iOS 游戏控制器](iphone-joystick.html)手册页以了解映射。
* 可通过专用 API 控制 Apple TV 遥控器操作模式：
    * `UnityEngine.Apple.TV.Remote.allowExitToHome`
    * `UnityEngine.Apple.TV.Remote.allowRemoteRotation`
    * `UnityEngine.Apple.TV.Remote.reportAbsoluteDpadValues`
    * `UnityEngine.Apple.TV.Remote.touchesEnabled`
* 可将另外两个无线 Made For iOS (MFi) 游戏控制器连接到 Apple TV 设备，从而有效地将该设备转换为游戏主机。游戏可能会像 iOS MFi 控制器一样使用这些控制器（如上所述），但应该仍能单独使用 Apple TV 遥控器玩游戏。目前，附加控制器的数量限制为两个；这是文档中明确指出的 tvOS 系统限制。

**警告：**由于当 `UnityEngine.Apple.TV.Remote.allowExitToHome` 设置为 **false** 时，Apple TV 遥控器“Menu”按钮被报告为[游戏杆按钮 0](iphone-joystick.html)，而默认输入管理器绑定的“Submit”虚拟按钮也映射到同一个[游戏杆按钮 0](iphone-joystick.html)，因此按下“Menu”按钮时，此按钮会触发 UI 元素上的操作。要解决此问题，请在[输入管理器](ConventionalGameInput.html)中删除或修改“Submit”虚拟按钮绑定。

## 设置通过 Apple TV 遥控器进行 Unity GUI 导航
* 在 Unity Editor 中打开 [输入管理器](ConventionalGameInput.html)。找到第一次出现的“Submit”虚拟输入，将其展开，然后将其“Alt Positive Button”更改为“joystick button 14”。
* 在场景中选择 EventSystem 游戏对象。在 Inspector 中，找到 EventSystem 组件，并使用应该接收初始焦点的 UI 游戏对象填充“First Selected”字段。可能需要选中“Standalone Input Module”组件中的“Force input module”标志。

进行这些设置后，即可在 Editor 中运行时通过键盘导航 UI，而在设备上运行时通过 Apple TV 遥控器轻扫并完全停止点击。

**注意**：在电视模拟器中运行时，Apple TV 遥控器导航无效。

## 将排行榜资源添加到 Xcode 项目
游戏中心 (Game Center) 要求为其原生排行榜 UI 提供自定义可视化资源。以下是有关如何在 Xcode 中设置这些资源的快速说明：

* 在 Xcode 项目中选择 Images.xcassets
* 右键单击下面列出的文件，然后从菜单中选择 **Game Center &gt; New AppleTV Leaderboard**。
* 添加图像。
* 选择 Leaderboard 并在右侧面板中选择 Edit View。找到“Identifier”字段并在其中输入您的排行榜 ID。
* 在这些修改之后，如果资源编译开始失败，请尝试在 Xcode 的“Build Settings”中禁用“On Demand Resources”。

<a name="OnDemandResources"></a> 
##实现按需加载资源 (On Demand Resources) 支持

对于应用程序可以保留的磁盘空间量，tvOS 有严格的要求。主应用程序安装包大小不能超过 200 MB。但是，其他可下载内容的大小上限要高得多（正在使用的资源最多可达 2GB，总共可下载内容最多可达 20GB）。Apple 建议对 tvOS 的可下载内容采用按需加载资源 (On Demand Resources, ODR)，因为此功能为 tvOS 提供了最佳的磁盘空间管理策略。Unity 通过资源包支持 ODR。在关于该主题的专用[博客文章](http://blogs.unity3d.com/2015/11/26/mastering-on-demand-resources-for-apple-platforms/)中可找到一份 ODR 实现指南。

## 已知限制
* 屏幕键盘仅限于单行输入。
* tvOS 模拟器不会将 Apple TV 遥控器模拟为游戏控制器，因此无法将其输入提供给游戏。
