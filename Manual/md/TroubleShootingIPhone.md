Troubleshooting on iOS devices
==============================


There are some situations with iOS where your game can work perfectly in the Unity editor but then doesn't work or maybe doesn't even start on the actual device.
The problems are often related to code or content quality. This section describes the most common scenarios.

The game stops responding after a while. Xcode shows "interrupted" in the status bar.
-------------------------------------------------------------------------------------

There are a number of reasons why this may happen. Typical causes include:

1. Scripting errors such as using uninitialized variables, etc.
1. Using 3rd party Thumb compiled native libraries. Such libraries trigger a known problem in the iOS SDK linker and might cause random crashes.
1. Using generic types with value types as parameters (e.g. List&lt;int&gt;, List&lt;SomeStruct&gt;, List&lt;SomeEnum&gt;) for serializable script properties.
1. Using reflection when managed code stripping is enabled.
1. Errors in the native plugin interface (the managed code method signature does not match the native code function signature).
Information from the XCode Debugger console can often help detect these problems (Xcode menu: __View &gt; Debug Area &gt; Activate Console__).

The Xcode console shows "Program received signal: &#8220;SIGBUS&#8221; or EXC_BAD_ACCESS error.
-----------------------------------------------------------------------------------------------

This message typically appears on iOS devices when your application receives a NullReferenceException. There are two ways to figure out where the fault happened: 

####Managed stack traces
Unity includes software-based handling of the NullReferenceException. The AOT compiler includes quick checks for null references each time a method or variable is accessed on an object. This feature affects script performance, which is why it is enabled only for development builds (enable the _script debugging_ option in build settings dialog).
If everything was done right and the fault actually is occurring in .NET code then you won't see EXC_BAD_ACCESS anymore. Instead, the .NET exception text will be printed in the Xcode console (or else your code will just handle it in a "catch" statement). Typical output might be:


````
Unhandled Exception: System.NullReferenceException: A null value was found where an object instance was required.
  at DayController+$handleTimeOfDay$121+$.MoveNext () [0x0035a] in DayController.js:122 
````

This indicates that the fault happened in the handleTimeOfDay method of the DayController class, which works as a coroutine. Also, if it is script code then you will generally be told the exact line number (e.g. "DayController.js:122"). The offending line might be something like the following:


````
 Instantiate(_imgwww.assetBundle.mainAsset);
````

This might happen if, say, the script accesses an asset bundle without first checking that it was downloaded correctly.

####Native stack traces
Native stack traces are a much more powerful tool for fault investigation but using them requires some expertise. Also, you generally can't continue after these native (hardware memory access) faults happen. To get a native stack trace, type __bt all__ into the Xcode Debugger Console. Carefully inspect the printed stack traces; they may contain hints about where the error occurred. You might see something like:


````
...
Thread 1 (thread 11523): 

1. 0 0x006267d0 in m_OptionsMenu_Start ()
1. 1 0x002e4160 in wrapper_runtime_invoke_object_runtime_invoke_void__this___object_intptr_intptr_intptr ()
1. 2 0x00a1dd64 in mono_jit_runtime_invoke (method=0x18b63bc, obj=0x5d10cb0, params=0x0, exc=0x2fffdd34) at /Users/mantasp/work/unity/unity-mono/External/Mono/mono/mono/mini/mini.c:4487
1. 3 0x0088481c in MonoBehaviour::InvokeMethodOrCoroutineChecked ()
...
````
Firstly, you should find the stack trace for **"Thread 1"**, which is the main thread. The very first lines of the stack trace will point to the place where the error occurred. In this example, the trace indicates that the NullReferenceException happened inside the _OptionsMenu_ script's _Start_ method. Looking carefully at this method implementation would reveal the cause of the problem. Typically, NullReferenceExceptions happen inside the _Start_ method when incorrect assumptions are made about initialization order.
In some cases only a partial stack trace is seen on the Debugger Console:


````
Thread 1 (thread 11523): 

1. 0 0x0062564c in start ()
````
This indicates that native symbols were stripped during the Release build of the application. The full stack trace can be obtained with the following procedure:

* Remove application from device.
* Clean all targets.
* Build and run.
* Get stack traces again as described above.

EXC_BAD_ACCESS starts occurring when an external library is linked to the Unity iOS application.
------------------------------------------------------------------------------------------------

This usually happens when an external library is compiled with the ARM Thumb instruction set. Currently, such libraries are not compatible with Unity. The problem can be solved easily by recompiling the library without Thumb instructions. You can do this for the library's Xcode project with the following steps:

* In Xcode, select _View_ &gt; _Navigators_ &gt; _Show Project Navigator_ from the menu
* Select the _Unity-iPhone_ project, activate _Build Settings_ tab
* In the search field enter: _Other C Flags_
* Add _-mno-thumb_ flag there and rebuild the library.

If the library source is not available you should ask the supplier for a non-thumb version of the library.

The Xcode console shows "WARNING -&gt; applicationDidReceiveMemoryWarning()" and the application crashes immediately afterwards
-------------------------------------------------------------------------------------------------------------------------------

Sometimes, you might see a message like _Program received signal: "0"_.
This warning message is often not fatal and merely indicates that iOS is low on memory and is asking applications to free up some memory. Typically, background processes like Mail will free some memory and your application can continue to run. However, if your application continues to use memory or ask for more, the OS will eventually start killing applications and yours could be one of them. Apple does not document what memory usage is safe, but empirical observations show that applications using less than 50% of all device RAM do not have major memory usage problems.
The main metric you should rely on is how much RAM your application uses. Your application memory usage consists of three major components:

* Application code (the OS needs to load and keep your application code in RAM, but some of it might be discarded if really needed)
* Native heap (used by the engine to store its state, your assets, etc. in RAM)
* Managed heap (used by your Mono runtime to keep C# objects)
* GLES driver memory pools: textures, framebuffers, compiled shaders, etc.
Your application memory usage can be tracked by two Xcode Instruments tools: __Activity Monitor__ and __Allocations__. You can start from the Xcode Run menu: __Product &gt; Profile__ and then select specific tool or via __Xcode &gt; Open Developer Tools &gt; Instruments__. __Activity Monitor__ tool shows all process statistics including __Real memory__ which can be regarded as the total amount of RAM used by your application. **Note:** OS and device HW version combination might noticeably affect memory usage numbers, so you should be careful when comparing numbers obtained on different devices.


![](../uploads/Main/ActivityMonitor.png) 

**Note:** The [internal profiler](iphone-InternalProfiler.html) shows only the heap allocated by .NET scripts. Total memory usage can be determined via Xcode Instruments as shown above. This figure includes parts of the application binary, some standard framework buffers, Unity engine internal state buffers, the .NET runtime heap (number printed by internal profiler), GLES driver heap and some other miscellaneous stuff. 


The other tool displays all allocations made by your application and includes both native heap and managed heap statistics. The important statistic is the __Net bytes__ value.


![](../uploads/Main/ObjectAlloc.png) 

To keep memory usage low:

* Reduce the application binary size by using the strongest iOS stripping options, and avoid unnecessary dependencies on different .NET libraries. See the [player settings](class-PlayerSettings.html) and [player size optimization](iphone-playerSizeOptimization.html) manual pages for further details.
* Reduce the size of your content. Use PVRTC compression for textures and use low poly models. See the manual page about [reducing file size](ReducingFilesize.html) for more information.
* Don't allocate more memory than necessary in your scripts. Track mono heap size and usage with the [internal profiler](iphone-InternalProfiler.html).

Querying the OS about the amount of free memory may seem like a good idea to evaluate how well your application is performing. However, the free memory statistic is likely to be unreliable since the OS uses a lot of dynamic buffers and caches. The only reliable approach is to keep track of memory consumption for your application and use that as the main metric. Pay attention to how the graphs from the tools described above change over time, especially after loading new levels.

The game runs correctly when launched from Xcode but crashes while loading the first level when launched manually on the device.
--------------------------------------------------------------------------------------------------------------------------------

There could be several reasons for this. You need to inspect the device logs to get more details. Connect the device to your Mac, launch Xcode and select __Window &gt; Devices and Simulators__ from the menu. Select your device in the window's left toolbar, then click on the __Show the device console__ button and review the latest messages carefully. Additionally, you may need to investigate crash reports. You can find out how to obtain crash reports here: [http://developer.apple.com/iphone/library/technotes/tn2008/tn2151.html](https://developer.apple.com/library/content/technotes/tn2151/_index.html).

The Xcode Organizer console contains the message "killed by SpringBoard".
-------------------------------------------------------------------------

There is a poorly-documented time limit for an iOS application to render its first frames and process input. If your application exceeds this limit, it will be killed by SpringBoard. This may happen in an application with a first scene which is too large, for example. To avoid this problem, it is advisable to create a small initial scene which just displays a splash screen, waits a frame or two with __yield__ and then starts loading the real scene. This can be done with code as simple as the following:


````
IEnumerator Start() {
    yield return new WaitForEndOfFrame();
// Do not forget using UnityEngine.SceneManagement directive
    SceneManager.LoadScene("Test");
}
````

Type.GetProperty() or Type.GetValue() cause crashes on the device
----------------------------------------------------------------

Currently __Type.GetProperty()__ and __Type.GetValue()__ are supported only for the __.NET 2.0 Subset__ profile. You can select the .NET API compatibility level in the [Player Settings](class-PlayerSettings.html). 

**Note:** __Type.GetProperty()__ and __Type.GetValue()__ might be incompatible with managed code stripping and might need to be excluded (you can supply a custom non-strippable type list during the stripping process to accomplish this). For further details, see the [iOS player size optimization guide](iphone-playerSizeOptimization.html).

The game crashes with the error message "ExecutionEngineException: Attempting to JIT compile method 'SometType`1&lt;SomeValueType&gt;:.ctor ()' while running with --aot-only."
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

The Mono .NET implementation for iOS is based on AOT (ahead of time compilation to native code) technology, which has its limitations. It compiles only those generic type methods (where a value type is used as a generic parameter) which are explicitly used by other code. When such methods are used only via reflection or from native code (i.e. the serialization system) then they get skipped during AOT compilation. The AOT compiler can be hinted to include code by adding a dummy method somewhere in the script code. This can refer to the missing methods and so get them compiled ahead of time.


````
void _unusedMethod() {
    var tmp = new SomeType<SomeValueType>();
}
````
**Note:** Value types are basic types, enums and structs.

Various crashes occur on the device when a combination of System.Security.Cryptography and managed code stripping is used
-------------------------------------------------------------------------------------------------------------------------

.NET Cryptography services rely heavily on reflection and so are not compatible with managed code stripping since this involves static code analysis. Sometimes the easiest solution to the crashes is to exclude the whole __System.Security.Crypography__ namespace from the stripping process.

The stripping process can be customized by adding a custom __link.xml__ file to the _Assets_ folder of your Unity project. This specifies which types and namespaces should be excluded from stripping. Further details can be found in the [iOS player size optimization guide](iphone-playerSizeOptimization.html).

###link.xml


````
<linker>
       <assembly fullname="mscorlib">
               <namespace fullname="System.Security.Cryptography" preserve="all"/>
       </assembly>
</linker>
````

###Application crashes when using System.Security.Cryptography.MD5 with managed code stripping
You should consider the above advice or try working around this problem by adding extra references to specific classes to your script code:


````
object obj = new MD5CryptoServiceProvider();
````

"Ran out of trampolines of type 0/1/2" runtime error
----------------------------------------------------

This error usually happens if you use lots of recursive generics. You can hint to the AOT compiler to allocate more trampolines of type 0, type 1 or type 2. Additional AOT compiler command line options can be specified in the _Other Settings_ section of the [Player Settings](class-PlayerSettings.html). For type 1 trampolines, specify `nrgctx-trampolines=ABCD`, where ABCD is the number of new trampolines required (i.e. 4096). For type 2 trampolines specify `nimt-trampolines=ABCD` and for type 0 trampolines specify `ntrampolines=ABCD`.

After upgrading Xcode Unity iOS, runtime fails with message "You are using Unity iPhone Basic. You are not allowed to remove the Unity splash screen from your game"
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

With some latest Xcode releases there were changes introduced in PNG compression and optimization tool. These changes might cause false positives in Unity iOS runtime checks for splash screen modifications. If you encounter such problems try upgrading Unity to the latest publicly available version. If this does not help, you might consider the following workaround:

* Replace your Xcode project from scratch when building from Unity (instead of appending it)
* Delete already installed projects from device
* Clean project in Xcode (_Product_ &gt; _Clean_)
* Clear Xcode's Derived Data folders (_Xcode_ &gt; _Preferences_ &gt; _Locations_)

If this still does not help try disabling PNG re-compression in Xcode:

* Open your Xcode project
* Select _Unity-iPhone_ project
* Select __Build Settings__ tab 
* Look for __Compress PNG files__ option and set to __NO__

WWW downloads are working fine in Unity Editor and on Android, but not on iOS
-----------------------------------------------------------------------------

The most common mistake is to assume that WWW downloads are always happening on a separate thread. On some platforms this might be true, but you should not take it for granted. Best way to track WWW status is either to use the _yield_ statement or check status in _Update_ method. You should **not** use busy _while_ loops for that.

"PlayerLoop called recursively!" error occurs when using Cocoa via a native function called from a script
---------------------------------------------------------------------------------------------------------

Some operations with the UI will result in iOS redrawing the window immediately (the most common example is adding a UIView with a UIViewController to the main UIWindow). If you call a native function from a script, it will happen inside Unity's PlayerLoop, resulting in PlayerLoop being called recursively. In such cases, you should consider using the [performSelectorOnMainThread](http://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html#/apple_ref/occ/instm/NSObject/performSelectorOnMainThread:withObject:waitUntilDone:) method with _waitUntilDone_ set to false. It will inform iOS to schedule the operation to run between Unity's PlayerLoop calls.

Profiler or Debugger unable to see game running on iOS device
-------------------------------------------------------------


* Check that you have built a Development build, and checked the _Script Debugging_ and _Autoconnect Profiler_ boxes (as appropriate).
* The application running on the device will make a multicast broadcast to 225.0.0.222 on UDP port 54997. Check that your network settings allow this traffic. Then, the profiler will make a connection to the remote device on a port in the range 55000 - 55511 to fetch profiler data from the device. These ports will need to be open for UDP access.

Missing DLLs
------------

If your application runs ok in editor but you get errors in your iOS project this may be caused by missing DLLs (e.g. I18N.dll, I19N.West.dll). In this case, try copying those dlls from within the Unity.app to your project's _Assets\Plugins_ folder. The location of the DLLs within the unity app is:
 _Unity.app\Contents\Frameworks\Mono\lib\mono\unity_ 
You should then also check the stripping level of your project to ensure the classes in the DLLs aren't being removed when the build is optimised. Refer to the [iOS Optimisation Page](iphone-playerSizeOptimization.html) for more information on iOS Stripping Levels.

Xcode Debugger console reports: ExecutionEngineException: Attempting to JIT compile method '(wrapper native-to-managed) Test:TestFunc (int)' while running with --aot-only
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Typically, such a message is received when the managed function delegate is passed to the native function, but the required wrapper code wasn't generated when the application was built. You can help AOT compiler by hinting which methods will be passed as delegates to the native code. This can be done by adding the _MonoPInvokeCallbackAttribute_ custom attribute. Currently, only static methods can be passed as delegates to the native code.

Sample code:


````
using UnityEngine;
using System.Collections;
using System;
using System.Runtime.InteropServices;
using AOT;

public class NewBehaviourScript : MonoBehaviour {
	[DllImport ("__Internal")]
	private static extern void DoSomething (NoParamDelegate del1, StringParamDelegate del2);

	delegate void NoParamDelegate ();
	delegate void StringParamDelegate (string str);
	
	[MonoPInvokeCallback(typeof(NoParamDelegate))]
	public static void NoParamCallback() {
		Debug.Log ("Hello from NoParamCallback");
	}
	
	[MonoPInvokeCallback(typeof(StringParamDelegate))]
	public static void StringParamCallback(string str) {
		Debug.Log(string.Format("Hello from StringParamCallback {0}", str));
	}

	// Use this for initialization
	void Start() {
		DoSomething(NoParamCallback, StringParamCallback);
	}
}
````

Xcode throws compilation error: "ld : unable to insert branch island. No insertion point available. for architecture armv7", "clang: error: linker command failed with exit code 1 (use -v to see invocation)"
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

This error usually means there is just too much code in single module. Typically, it is caused by having lots of script code or having big external .NET assemblies included into build. And enabling script debugging might make things worse, because it adds quite few additional instructions to each function, so it is easier to hit that limit.

Enabling managed code stripping in [player settings](class-PlayerSettings.html) might help with this problem, especially if big external .NET assemblies are involved. But if the issue persists then the best solution is to split user script code into multiple assemblies. The easiest way to this is move some code to _Plugins_ folder. Code at this location is put to a different assembly. Also, check the information about how [special folder names](ScriptCompileOrderFolders.html) affect script compilation.

---

* <span class="page-edit">2018-06-14  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
