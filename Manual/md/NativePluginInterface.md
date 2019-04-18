#低级原生插件接口

除了基本脚本接口之外，Unity 中的[原生代码插件](Plugins.html)还可以在发生某些事件时接收回调。这主要用于实现插件中的低级渲染，并使低级渲染能够与 Unity 的多线程渲染一起使用。

定义 Unity 所公开的接口的头文件随 Editor 一起提供。


## 接口注册表

插件应导出 `UnityPluginLoad` 和 `UnityPluginUnload` 函数来处理主要的 Unity 事件。请参阅 `IUnityInterface.h` 以查看正确的签名。提供插件的 `IUnityInterfaces` 是为了访问更多的 Unity API。

````
# include "IUnityInterface.h"
# include "IUnityGraphics.h"
// Unity 插件加载事件
extern "C" void UNITY_INTERFACE_EXPORT UNITY_INTERFACE_API
    UnityPluginLoad(IUnityInterfaces* unityInterfaces)
{
    IUnityGraphics* graphics = unityInterfaces->Get<IUnityGraphics>();
}
````


## 访问图形设备

插件可通过获取 `IUnityGraphics` 接口来访问通用图形设备功能。在早期版本的 Unity 中，必须导出 `UnitySetGraphicsDevice` 函数才能接收图形设备上的事件的相关通知。从 Unity 5.2 开始，新的 IUnityGraphics 接口（位于 `IUnityGraphics.h` 中）提供了一种注册回调的方式。

````
# include "IUnityInterface.h"
# include "IUnityGraphics.h"
    
static IUnityInterfaces* s_UnityInterfaces = NULL;
static IUnityGraphics* s_Graphics = NULL;
static UnityGfxRenderer s_RendererType = kUnityGfxRendererNull;
    
// Unity 插件加载事件
extern "C" void UNITY_INTERFACE_EXPORT UNITY_INTERFACE_API
    UnityPluginLoad(IUnityInterfaces* unityInterfaces)
{
    s_UnityInterfaces = unityInterfaces;
    s_Graphics = unityInterfaces->Get<IUnityGraphics>();
        
    s_Graphics->RegisterDeviceEventCallback(OnGraphicsDeviceEvent);
        
    // 在插件加载时运行 OnGraphicsDeviceEvent(initialize)
    // 在图形设备已初始化的情况下不错过该事件
    OnGraphicsDeviceEvent(kUnityGfxDeviceEventInitialize);
}
    
// Unity 插件卸载事件
extern "C" void UNITY_INTERFACE_EXPORT UNITY_INTERFACE_API
    UnityPluginUnload()
{
    s_Graphics->UnregisterDeviceEventCallback(OnGraphicsDeviceEvent);
}
    
static void UNITY_INTERFACE_API
    OnGraphicsDeviceEvent(UnityGfxDeviceEventType eventType)
{
	switch (eventType)
	{
	    case kUnityGfxDeviceEventInitialize:
		{
			s_RendererType = s_Graphics->GetRenderer();
			//TODO：用户初始化代码
			break;
		}
	    case kUnityGfxDeviceEventShutdown:
		{
			s_RendererType = kUnityGfxRendererNull;
			//TODO：用户关闭代码
			break;
		}
	case kUnityGfxDeviceEventBeforeReset:
		{
		    //TODO：用户 Direct3D 9 代码
			break;
		}
	case kUnityGfxDeviceEventAfterReset:
		{
		    //TODO：用户 Direct3D 9 代码
			break;
		}
	};
}
````    

## 渲染线程上的插件回调

如果平台和可用 CPU 数允许，则在 Unity 中的渲染可以是多线程的。当使用多线程渲染时，执行这些渲染 API 命令的线程完全独立于运行 MonoBehaviour 脚本的线程。因此，您的插件并非总是可以立即开始进行渲染，因为它可能会干扰渲染线程此时正在进行的工作。

为了从插件中进行**任何**渲染，应从脚本中调用 [GL.IssuePluginEvent](../ScriptReference/GL.IssuePluginEvent.html)。这将导致从渲染线程调用提供的本机函数。例如，如果从摄像机的 OnPostRender 函数调用 GL.IssuePluginEvent，则在该摄像机已完成渲染后，您将立即获得插件回调。

在 `IUnityGraphics.h` 中提供了 `UnityRenderingEvent` 回调的签名。
原生插件代码示例：

````
// 用于处理特定渲染事件的插件函数
static void UNITY_INTERFACE_API OnRenderEvent(int eventID)
{
    //TODO：用户渲染代码
}
    
// 自由定义的函数，用于将回调传递给特定于插件的脚本
extern "C" UnityRenderingEvent UNITY_INTERFACE_EXPORT UNITY_INTERFACE_API
    GetRenderEventFunc()
{
    return OnRenderEvent;
}
````
    
托管插件代码示例：
    
````
# if UNITY_IPHONE &amp;&amp; !UNITY_EDITOR
[DllImport ("__Internal")]
# else
[DllImport("RenderingPlugin")]
# endif
private static extern IntPtr GetRenderEventFunc();
	
// 对要在渲染线程上调用的特定回调进行排队
GL.IssuePluginEvent(GetRenderEventFunc(), 1);
````

现在还可通过 [CommandBuffer.IssuePluginEvent](../ScriptReference/Rendering.CommandBuffer.IssuePluginEvent.html) 将此类回调添加到 CommandBuffer。

## 使用 OpenGL 图形 API 的插件

有两种 OpenGL 对象：跨 OpenGL 上下文共享的对象（纹理、缓冲、渲染缓冲、采样器、查询、着色器和程序对象）和每个 OpenGL 的上下文对象（顶点数组、帧缓冲、程序管线、变换反馈和同步对象）。

Unity 使用多个 OpenGL 上下文。在初始化和关闭 Editor 和播放器时，我们依赖于**主**上下文，但我们使用专用上下文进行渲染。因此，在 `kUnityGfxDeviceEventInitialize` 和 `kUnityGfxDeviceEventShutdown` 事件期间无法创建每个上下文的对象。

例如，原生插件在 `kUnityGfxDeviceEventInitialize` 事件期间无法创建顶点数组对象并在 `UnityRenderingEvent` 回调中使用该对象，因为活动上下文不是顶点数组对象创建期间使用的上下文。

## 示例


Bitbucket 上提供了一个低级渲染插件的示例：**[bitbucket.org/Unity-Technologies/graphicsdemos](https://bitbucket.org/Unity-Technologies/graphicsdemos)**（NativeRenderingPlugin 文件夹）。该示例展示了两个方面：


* 在完成所有常规渲染后，从 C++ 代码渲染旋转三角形。
* 从 C++ 代码填充程序化纹理（使用 Texture.GetNativeTexturePtr 来访问该纹理）。

该项目适用于：

* 装有 Direct3D 9、Direct3D 11、Direct3D 12 和 OpenGL 的 Windows (Visual Studio 2015)。
* 装有 Metal 和 OpenGL 的 Mac OS X (Xcode)。
* 装有 Direct3D 11 和 Direct3D 12 的通用 Windows 平台。
* WebGL

---

<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
