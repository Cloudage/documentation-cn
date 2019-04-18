#平台相关的编译


Unity 包括名为__平台相关的编译 (Platform Dependent Compilation)__ 的功能。此功能包括一些预处理器指令，可让您对脚本进行分区，从而为受支持平台之一专门编译和执行一段代码。

您可以在 Unity Editor 中运行此代码，这样便可专门为目标平台编译代码并在 Editor 中对其进行测试！


##平台 #define 指令


Unity 支持对脚本使用的平台 #define 指令如下：


|__属性：__ |__功能：__ |
|:---|:---|
|__UNITY\_EDITOR__ |用于从游戏代码中调用 Unity Editor 脚本的 #define 指令。|
|__UNITY\_EDITOR\_WIN__ |用于 Windows 上的 Editor 代码的 #define 指令。 |
|__UNITY\_EDITOR\_OSX__ |用于 Mac OS X 上的 Editor 代码的 #define 指令。 |
|__UNITY\_STANDALONE\_OSX__ |用于专门为 Mac OS X（包括 Universal、PPC 和 Intel 架构）编译/执行代码的 #define 指令。|
|__UNITY\_STANDALONE\_WIN__ |用于专门为 Windows 独立平台应用程序编译/执行代码的 #define 指令。|
|__UNITY\_STANDALONE\_LINUX__ |用于专门为 Linux 独立平台应用程序编译/执行代码的 #define 指令。 |
|__UNITY\_STANDALONE__ |用于为任何独立平台（Mac OS X、Windows 或 Linux）编译/执行代码的 #define 指令。 |
|__UNITY\_WII__ |用于为 Wii 游戏主机编译/执行代码的 #define 指令。|
|__UNITY\_IOS__ |用于为 iOS 平台编译/执行代码的 #define 指令。|
|__UNITY\_IPHONE__ |已弃用。改用 __UNITY\_IOS__。 |
|__UNITY\_ANDROID__ |用于 Android 平台的 #define 指令。|
|__UNITY\_PS4__ |用于运行 PlayStation 4 代码的 #define 指令。|
|__UNITY\_XBOXONE__ |用于执行 Xbox One 代码的 #define 指令。|
|__UNITY\_TIZEN__ |用于 Tizen 平台的 #define 指令。|
|__UNITY\_TVOS__ |用于 Apple TV 平台的 #define 指令。|
|__UNITY\_WSA__ |用于通用 Windows 平台的 #define 指令。此外，根据 .NET Core 和使用 .NET 脚本后端来编译 C# 文件时会定义 __NETFX\_CORE__。|
|__UNITY\_WSA_10_0__ |用于通用 Windows 平台的 #define 指令。此外，根据 .NET Core 来编译 C# 文件时会定义 __WINDOWS_UWP__。|
|__UNITY\_WINRT__ | 与 __UNITY\_WSA__ 相同。 |
|__UNITY\_WINRT_10_0__ |等效于 __UNITY\_WSA_10_0__ |
|__UNITY\_WEBGL__ |用于 WebGL 的 #define 指令。|
|__UNITY\_FACEBOOK__ |用于 Facebook 平台（WebGL 或 Windows 独立平台）的 #define 指令。|
|__UNITY\_ADS__ |用于从游戏代码中调用 Unity Ads 方法的 #define 指令。5.2 及更高版本。|
|__UNITY\_ANALYTICS__ |用于从游戏代码中调用 Unity Analytics 方法的 #define 指令。5.2 及更高版本。|
|__UNITY\_ASSERTIONS__ |用于断言控制过程的 #define 指令。|



从 Unity 2.6.0 开始，可有选择地编译代码。可用选项取决于所使用的 Editor 版本。
如果版本号为 __X.Y.Z__（例如，2.6.0），Unity 将使用以下格式公开三个全局 #define 指令：__UNITY\_X__、__UNITY\_X\_Y__ 和 __UNITY\_X\_Y\_Z__。

以下是 Unity 5.0.1 中公开的 #define 指令的示例：

| | |
|:---|:---|
|__UNITY\_5__ |用于 Unity 5 发行版的 #define 指令，在每个 5.X.Y 版本中公开。|
|__UNITY\_5\_0__ |用于 Unity 5.0 主要版本的 #define 指令，在每个 5.0.Z 版本中公开。|
|__UNITY\_5\_0\_1__ |用于 Unity 5.0.1 次要版本的 #define 指令。|


从 Unity 5.3.4 开始，可根据编译或执行给定代码部分所需的最早 Unity 版本来有选择地编译代码。如果采用与上面相同的版本格式 (__X.Y.Z__)，Unity 将以 __UNITY\_X\_Y\_OR\_NEWER__ 格式公开一个可用于此目的的全局 #define。

支持的 #define 指令为：

| | |
|:---|:---|
|__ENABLE\_MONO__ |用于 Mono 的脚本后端 #define。|
|__ENABLE\_IL2CPP__ |用于 IL2CPP 的脚本后端 #define。|
|__ENABLE\_DOTNET__ |用于 .NET 的脚本后端 #define。|
|__NETFX\_CORE__ |在 .NET 上根据 .NET Core 类库构建脚本时定义。|
|__NET\_2\_0__ |在 Mono 和 IL2CPP 上根据 .NET 2.0 API 兼容性级别构建脚本时定义。|
|__NET\_2\_0\_SUBSET__ |在 Mono 和 IL2CPP 上根据 .NET 2.0 Subset API 兼容性级别构建脚本时定义。|
|__NET\_4\_6__ |在 Mono 和 IL2CPP 上根据 .NET 4.x API 兼容性级别构建脚本时定义。|
|__NET_STANDARD_2_0__ |在 Mono 和 IL2CPP 上根据 .NET 标准 2.0 API 兼容性级别构建脚本时定义。|
|__ENABLE\_WINMD\_SUPPORT__ |在 IL2CPP 和 .NET 上启用 Windows 运行时支持时定义。有关更多详细信息，请参阅 [Windows 运行时支持](IL2CPP-WindowsRuntimeSupport.html)。|




可使用 DEVELOPMENT_BUILD #define 来确定脚本是否正在启用了“Development Build”选项的情况下构建的播放器中运行。

还可根据脚本后端有选择地编译代码。

##测试预编译的代码


下面是如何使用预编译代码的示例。该示例根据为目标构建选择的平台打印一条消息。

首先，通过 __File &gt; Build Settings__ 选择要测试代码的平台。随后将显示 __Build Settings__ 窗口；从此处选择目标平台。

![在 Build Settings 窗口中选择“PC, Mac & Linux”作为目标平台](../uploads/Main/BuildSettings.png)

选择要测试预编译代码的平台，然后单击 __Switch Platform__ 向 Unity 告知您所需的目标平台。

创建脚本并复制/粘贴以下代码：


````
// JS
function Awake() {
  #if UNITY_EDITOR
    Debug.Log("Unity Editor");
  #endif
	
  #if UNITY_IPHONE
    Debug.Log("Iphone");
  #endif

  #if UNITY_STANDALONE_OSX
    Debug.Log("Stand Alone OSX");
  #endif

  #if UNITY_STANDALONE_WIN
    Debug.Log("Stand Alone Windows");
  #endif	
}


// C#
using UnityEngine;
using System.Collections;

public class PlatformDefines : MonoBehaviour {
  void Start () {

    #if UNITY_EDITOR
      Debug.Log("Unity Editor");
    #endif
    
    #if UNITY_IOS
      Debug.Log("Iphone");
    #endif

    #if UNITY_STANDALONE_OSX
	Debug.Log("Stand Alone OSX");
    #endif

    #if UNITY_STANDALONE_WIN
      Debug.Log("Stand Alone Windows");
    #endif

  }			 
}


````

要测试代码，请单击 __Play Mode__。通过在 Unity 控制台中检查相关消息来确认代码是否正常工作，具体取决于选择的平台；例如，如果选择 __iOS__，则消息“Iphone”设置为显示在控制台中。

在 C# 中，可使用 `CONDITIONAL` 属性，这是一种更简洁、更不容易出错的函数剥离方式。请参阅 [ConditionalAttribute 类](https://msdn.microsoft.com/en-us/library/system.diagnostics.conditionalattribute(v=vs.110).aspx)以了解更多信息。
请注意，常见的 Unity 回调（例如：Start()、Update()、LateUpdate()、FixedUpdate()、Awake()）不受此属性的影响，因为它们是直接从引擎调用的，并且出于性能原因，此属性不会考虑它们。

除了基本的 `#if` 编译器指令外，还可在 C# 中使用多路测试：


````

# if UNITY_EDITOR
    Debug.Log("Unity Editor");

# elif UNITY_IOS
    Debug.Log("Unity iPhone");

# else
    Debug.Log("Any other platform");

# endif


````


##平台自定义 #define 指令


您也可以向内置的 #define 指令集合中添加您自己的指令。打开 [Player Settings](class-PlayerSettings.html) 的 __Other Settings__ 面板，并导航到 __Scripting Define Symbols__ 文本框。


![](../uploads/Main/ScriptDefines.png) 

输入要为该特定平台定义的符号名称，以分号分隔。随后可以将这些符号用作 `#if` 指令中的条件，就像内置条件一样。

##全局自定义 #define 指令


您可以定义自己的预处理器指令来控制在编译时包含的代码。为此，必须将包含额外指令的文本文件添加到 __Assets__ 文件夹。文件名取决于您使用的语言。扩展名为 __.rsp__：


| | |
|:---|:---|
|C#（播放器和 Editor 脚本）|&lt;项目路径&gt;/Assets/mcs.rsp |
|UnityScript|&lt;项目路径&gt;/Assets/us.rsp |


举例来说，如果在 __mcs.rsp__ 文件中包含单行 ``-define:UNITY_DEBUG``，则 #define 指令 `UNITY_DEBUG` 将作为 C# 脚本的全局 #define 指令存在，但 Editor 脚本除外。

每次对 __.rsp__ 文件进行更改时，都需要重新编译才能使更改生效。可通过更新或重新导入单个脚本（.js 或 .cs）文件来完成此操作。

__注意__

如果只想修改全局 #define 指令，请使用 [Player Settings](class-PlayerSettings.html) 中的 __Scripting Define Symbols__，因为此选项涵盖了所有编译器。如果选择 __.rsp__ 文件，则需要为 Unity 使用的每个编译器提供一个文件，并且您不知道何时使用一个或另一个编译器。

包含在 Editor 安装文件夹中的 __mcs__ 应用程序的“Help”部分中描述了 __.rsp__ 文件的使用方法。可通过运行 `mcs -help` 获取更多信息。

请注意，__.rsp__ 文件需要与正在调用的编译器匹配。例如：

* 以任何播放器或 Editor 为目标时，__mcs__ 与 `mcs.rsp` 一起使用，
* 以 MS 编译器为目标时，__csc__ 与 `csc.rsp` 一起使用，等等

---

* <span class="page-edit">2018-03-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>

* <span class="page-history">删除了三星电视支持。</span>
