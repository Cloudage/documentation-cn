# 性能分析

谈到性能就不得不提如何监测性能。首先监测应用的性能，分析并发现其性能消耗点，然后根据项目的技术和资源架构对性能进行分析。

**注意**：本章节提及的原生代码性能分析跟踪中使用的方法名称出自 Unity 5.3。方法名称在 Unity 的后续版本中可能会发生变更。

## 工具

Unity 开发者可使用多种工具进行性能分析。Unity 有一套内置工具，如 [CPU 性能分析器 (CPU Profiler)](ProfilerCPU.html)、[内存性能分析器 (Memory Profiler)](ProfilerMemory.html) 和新的 5.3 版内存分析器。

但是，最佳数据通常来自特定于平台的工具。这些工具包括：

* 针对 iOS 平台的：__Instruments__ 和 __XCode Frame Debugger__

* 针对 Android 平台的：__Snapdragon Profiler__

* 针对 Intel CPU/GPU 平台的：__VTune__ 和 __Intel GPA__

* For PS4: the __Razor__ suite and __VR Trace__

* 针对 Xbox 平台的：__Pix__ 工具

这些工具通常在能使用 [IL2CPP](IL2CPP.html) 生成 C++ 版项目的平台上利用率最高。这些原生代码版本提供了，在 Mono 下无法调用的透明的调用栈和高时域频度的方法调用。

Unity 已创建了使用 Instruments 来分析 iOS 游戏性能的基础指南；请参阅[使用 Instruments 进行性能分析 (Profiling with Instruments)](http://blogs.unity3d.com/2016/02/01/profiling-with-instruments/)。

## 解析启动跟踪

在查看启动时刻的跟踪记录时，需要关注两种关键方法。这两种方法是项目的配置、资源和代码可能影响启动时间的主要原因。

请注意，启动时间在不同平台上的表现方式不同。在大多数平台上，以静态启动画面的形式呈现给用户。

![](../uploads/Main/UnderstandingPerformanceinUnity-ProfilingSection_image_0.png) 

上图的截屏是 Instruments 跟踪的在 iOS 设备上运行的示例项目。在基于平台的 `startUnity` 方法中，请注意 `UnityInitApplicationGraphics` 和 `UnityLoadApplication` 方法。

`UnityInitApplicationGraphics` 执行大量内部工作，例如设置图形设备和初始化 Unity 的大量内部系统。此外，它还初始化资源系统 (Resources system)。为此，它必须加载资源系统包含的所有文件的索引。

每个“Resources”文件夹中的每个资源文件 (1)（注意：
	 这仅适用于项目“Assets”文件夹中名为“Resources”的文件夹，以及这些“Resources”文件夹中的所有子文件夹。）都作为资源系统的数据。因此，初始化资源系统所需的时间与“Resources”文件夹中的文件数量呈线性关系。

`UnityLoadApplication` 包含加载并初始化项目的第一个场景的方法。它包括反序列化并实例化显示第一个场景所需的所有数据，例如编译着色器、上传纹理和实例化游戏对象。此外，第一个场景中的所有 MonoBehaviour 都在此时执行 `Awake` 回调。

这就意味着，如果在项目的第一个场景中的 `Awake` 回调中存在任何执行时间很长的代码，那么该代码可能会导致项目的初始启动时间的延长。解决此问题的方法是删除这些运行速度慢的代码，或者在应用程序生命周期的其他地方执行该代码。

## 解析运行时跟踪

对于在初始化之后的性能分析跟踪而言，最需要关注的是 `PlayerLoop` 方法。这是 Unity 的主循环，该循环中的代码每帧运行一次。

![](../uploads/Main/UnderstandingPerformanceinUnity-ProfilingSection_image_1.png) 

上面的截屏来自 Unity 5.4 示例项目的性能分析运行结果，其中展示了 `PlayerLoop` 中几个最值得关注的方法。请注意，`PlayerLoop` 中的方法名称可能因 Unity 版本而异。

`PlayerRender` 是运行 Unity 渲染系统的方法。此方法进行的操作包括剔除对象、计算动态批次以及向 GPU 提交绘图指令。任何图像效果或基于渲染的脚本回调（例如 `OnWillRenderObject`）也在此时运行。通常情况下，当项目进行交互时，此方法应该对 CPU 时间的消耗最大。

`BaseBehaviourManager` 调用三个模板化版本的 `CommonUpdate`。这些调用的某些回调存在于当前场景中激活的游戏对象的 MonoBehaviour 中。

* `CommonUpdate<UpdateManager>` 调用 `Update` 回调

* `CommonUpdate<LateUpdateManager>` 调用 `LateUpdate` 回调

* `CommonUpdate<FixedUpdateManager>` 调用 `FixedUpdate`（如果物理系统已勾选）

通常情况下，`BaseBehaviourManager::CommonUpdate<UpdateManager>` 是最值得关注和检查的方法族，因为它是 Unity 项目中运行的大多数脚本代码的入口点。

还有其他几个需要关注的方法：

`UI::CanvasManager` 如果项目使用 Unity UI，它将调用几个不同的回调。包括 Unity UI 的批量计算和布局更新；这两种操作是 `CanvasManager` 出现在性能分析器中的最常见原因。

`DelayedCallManager::Update` 运行协程。该内容在本文档的“协程”章节中有更详细的介绍。

`PhysicsManager::FixedUpdate` 运行 PhysX 物理系统。这主要涉及运行 PhysX 的内部代码，并受当前场景中物理对象（例如刚体和碰撞体）数量的影响。但是，基于物理系统的回调也会出现在此处，例如 `OnTriggerStay` 和 `OnCollisionStay`。

如果项目使用的是 2D 物理设置，那么会在 `Physics2DManager::FixedUpdate` 下显示为一组与上面相似的调用。

## 解析脚本方法

在使用 IL2CPP 进行跨平台编译调用脚本时，应查找包含 `ScriptingInvocation` 对象的跟踪行。它是将 Unity 的内部原生代码转换到脚本运行时以便执行脚本代码的命令 (2)（注意：
	 从技术上讲，在经过 IL2CPP 编译后，C#/JS 脚本代码也会成为原生代码。但是，这种交叉编译后的代码的方法主要通过 IL2CPP 运行时框架来执行，与手写的 C++ 不太一样）。

![](../uploads/Main/UnderstandingPerformanceinUnity-ProfilingSection_image_2.png) 

上图的截屏是另一个来自 Unity 5.4 运行的示例项目的跟踪记录。嵌套在 `RuntimeInvoker_Void` 行下面的所有方法都是每帧执行一次的交叉编译 C# 脚本的一部分。

跟踪行相当易读：每一行都是原始类的名称，后跟下划线和原始方法的名称。对于此跟踪示例，可以看到 `EventSystem.Update`、`PlayerShooting.Update` 和其他几个 `Update` 方法。这些是 MonoBehaviour 中的常见的标准 Unity `Update` 回调。

展开这些方法的节点，可以准确发现其中的哪些方法正在消耗 CPU 时间。它包括项目中的其他脚本方法、Unity API 和 C# 库代码。

上面的跟踪记录显示了 `StandaloneInputModule.Process` 方法逐帧对整个 UI 进行射线投射判断，以便检测是否有任何触摸事件正在发生或激活了某些 UI 元素。主要消耗是迭代测试鼠标的位置是否在UI 元素的边界矩形内。

## 资源加载

CPU 跟踪记录中也可以识别出资源加载记录。标识资源加载的主要方法是 `SerializedFile::ReadObject`。此方法将二进制数据流（从文件）通过运行名为 `Transfer` 的方法连接到 Unity 的序列化系统中。可以在所有资源类型（例如纹理、MonoBehaviour 和粒子系统）上找到 `Transfer` 方法。

![](../uploads/Main/UnderstandingPerformanceinUnity-ProfilingSection_image_3.png) 

在上面的截屏中，正在加载场景文件。这需要 Unity 读取并反序列化场景中的所有资源，如 `SerializedFile::ReadObject` 中包含的各种调用 `Transfer` 的方法。

通常，如果在运行时期间看到性能不稳定，并且性能跟踪记录显示 `SerializedFile::ReadObject` 使用了大量时间，那么帧率降低的原因是由于资源加载。请注意，在大多数情况下，只有在通过 `SceneManager`、`Resources` 或 AssetBundle API 来请求同步的资源加载时，才能在主线程上找到 `SerializedFile::ReadObject`。

这种性能不稳问题的最常见的修正方式是，将资源加载异步进行（将资源消耗严重的 `ReadObject` 调用移到工作线程），或预加载某些大型资源。

请注意，在克隆对象（在跟踪记录中以 `CloneObject` 方法表示）时也会出现 `Transfer` 调用。如果在 `CloneObject` 调用下面出现 `Transfer` 调用，该调用是不会从存储中加载资源的。而是将旧对象的数据传输到新对象。为此，Unity 会序列化旧对象，并将结果数据反序列化为新对象。

###脚注

* (1) 这仅适用于项目 _Assets_ 文件夹中名为 _Resources_ 的文件夹，以及这些 _Resources_ 文件夹中的所有子文件夹。
* (2) 从技术上讲，经过 IL2CPP 编译后，C#/JS 脚本代码也会成为原生代码。但是，这种交叉编译后的代码主要通过 IL2CPP 运行时框架来执行方法，与手写的 C++ 不太一样。

---
* <span class="page-edit">2018-04-25 Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>



