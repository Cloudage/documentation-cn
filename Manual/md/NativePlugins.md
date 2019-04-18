#原生插件

Unity 广泛支持原生__插件__，即用 C、C++、Objective-C 等编写的原生代码库。插件允许游戏代码（用 Javascript 或 C# 编写）调用这些库中的函数。此功能允许 Unity 与中间件库或现有的 C/C++ 游戏代码集成。

为了使用原生插件，首先需要使用基于 C 的语言编写函数来访问所需的任何功能并将它们编译到库中。在 Unity 中，还需要创建一个 C# 脚本来调用本机库中的函数。

原生插件应提供一个简单的 C 接口供 C# 脚本随后向其他用户脚本公开。当某些低级渲染事件发生时（例如，创建图形设备时），Unity 也可以调用原生插件导出的函数，请参阅[原生插件接口](NativePluginInterface.html)页面以了解详细信息。


##示例

具有单个函数的非常简单的本机库可能具有如下所示的源代码：

````
	float FooPluginFunction () { return 5.0F; }
````

要从 Unity 中访问此代码，可使用如下代码：

````
	using UnityEngine;
		using System.Runtime.InteropServices;

		class SomeScript : MonoBehaviour {

		   #if UNITY_IPHONE
   
		   // 在 iOS 上，插件以静态方式链接到
		   //可执行文件中，因此我们必须使用 __Internal 作为
		   // 库名。
		   [DllImport ("__Internal")]

		   #else

		   // 其他平台会动态加载插件，因此
		   // 传递插件动态库的名称。
		   [DllImport ("PluginName")]
	
		   #endif

		   private static extern float FooPluginFunction ();

		   void Awake () {
			  // 在插件中调用 FooPluginFunction
			  // 并将 5 输出到控制台
			  print (FooPluginFunction ());
		   }
		}
````

请注意，使用 Javascript 时，需要使用以下语法，其中 DLLName 表示所编写的插件的名称，如果编写的是静态链接的本机代码，则需要使用“__Internal”：


````
	@DllImport (DLLName)
		static private function FooPluginFunction () : float {};
````

##创建原生插件

通常，插件是在目标平台上使用本机代码编译器构建的。由于插件函数使用基于 C 的调用接口，因此在使用 C++ 或 Objective-C 时必须避免名称错用问题。


##其他信息

* [原生插件接口](NativePluginInterface.html)（如果要在插件中进行渲染，则需要此接口）
* [Mono 互操作性与本机库](http://www.mono-project.com/Interop_with_Native_Libraries)
* [MSDN 上的 P-invoke 文档](http://msdn2.microsoft.com/en-us/library/fzhhdwae.aspx)
