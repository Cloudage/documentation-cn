性能分析
=========

初期步骤
-----------


Unity 依赖于 CPU（针对其 SIMD 部分进行了大量优化，如 x86 上的 SSE 或 ARM 上的 NEON）执行蒙皮、批处理、物理、用户脚本、粒子等。

GPU 用于着色器、绘制调用、图像效果。

###CPU 密集型还是 GPU 密集型


* 使用内部性能分析器检测 CPU 和 GPU 时间 (ms)

###帕累托分析
绝大多数问题 (80%) 是由几个主要原因 (20%) 产生的。可使用 Editor 性能分析器识别处理器密集程度最高的函数调用并首先对它们进行优化。通常，优化一些主要函数可以显著提高整体执行速度。

应确保仅在必要时才执行函数。例如，可使用 _OnBecameVisible_ 和 _OnBecameInvisible_ 等事件来检测无法看到对象的情况并避免更新该对象。可使用协程来调用需要定期更新但不需要每帧运行的代码：



````
// 每帧执行一些操作：
void Update () {
}

// 每 0.2 秒执行一些操作：
IEnumerator SlowUpdate () {
   while (true) {
      // 执行操作
      yield return new WaitForSeconds (0.2f);
   }
}


````

可使用 .NET _System.Threading.Thread_ 类在单独的线程上运行繁重的计算。因此可在多核上运行，但请注意，Unity API 不具有线程安全性；您需要缓冲输入和结果，并在主线程上读取和分配它们以便使用 Unity API 调用。

CPU 性能分析
-------------


###对用户代码进行性能分析

并非所有用户代码都显示在性能分析器中。但可使用 _Profiler.BeginSample_ 和 _Profiler.EndSample_ 使所需的用户代码显示在性能分析器中。

GPU 性能分析
-------------


Unity Editor 性能分析器目前无法显示 GPU 数据。我们正在与硬件制造商合作，使 Tegra 设备率先出现在 Editor 性能分析器中。

###适用于 iOS 的工具

* Unity 内部性能分析器（不是 Editor 性能分析器）。此性能分析器会显示整个场景的 GPU 时间。
* PowerVR PVRUniSCo 着色器分析器。请参阅下文。
* iOS：Xcode OpenGL ES Driver Instruments 仅能显示高级信息：
    * "Device Utilization %" - 总共用于渲染的 GPU 时间。&gt;95% 表示应用程序为 GPU 密集型。
    * "Renderer Utilization %" - 用于绘制像素的 GPU 时间。
    * "Tiler Utilization %" - 用于处理顶点的 GPU 时间。
    * "Split count" - 顶点数据不能完全装入分配的缓冲区时，帧分隔数。

PowerVR 是基于区块的延迟渲染器，因此不可能获得每个绘制调用的 GPU 时间。但是，可使用 Unity 的内置性能分析器（将结果打印到 Xcode 输出的性能分析器）获得整个场景的 GPU 时间。Apple 的工具目前只能显示 GPU 及其部件的繁忙程度，而不能给出以毫秒为单位的时间。

PVRUniSCo 提供整个着色器的周期，并估算着色器代码中每一行的
周期。Windows 和 Mac！但是，该工具始终无法与 Apple 的驱动程序做到完全匹配。仍然可作为一种很好的估算手段。

###适用于 Android 的工具

* Adreno (Qualcomm)
* NVPerfHUD (NVIDIA)
* PVRTune、PVRUniSCo (PowerVR)

在 Tegra 上，NVIDIA 提供了出色的性能工具，可提供您所需的所有数据（每次绘制调用的 GPU 时间、每个着色器的周期数、强制 2x2 纹理、空视图矩形），并可在 Windows、OSX 和 Linux 上运行。PerfHUD ES 不易与消费类设备配合使用，需要使用 NVIDIA 的开发板。

Qualcomm 提供出色的 Adreno Profiler（仅限 Windows），仅适用于 Windows，但可与消费类设备配合使用！该工具提供时间轴图、帧捕获、帧调试、API 调用、着色器分析器和实时编辑功能。

###与图形相关的 CPU 分析

内部性能分析器可提供每个模块的概况：

* 在 OpenGL ES API 中花费的时间
* 批处理效率
* 蒙皮、动画、粒子


性能分析器端口
--------------

Unity Profiler 使用的端口：

+	多播端口：54998
+	监听端口：55000 - 55511
+	多播（单元测试）：55512 - 56023

应可从网络节点内访问这些端口。也就是说，您尝试分析性能的设备应能够在装有启用了性能分析器的 Unity Editor 的计算机上看到这些端口。




内存
------


有两种类型的内存：Mono 内存和 Unity 内存。

###Mono 内存

Mono 内存处理脚本对象以及 Unity 对象（游戏对象、资源、组件等）的包装器。当分配量超出可用内存或执行 _System.GC.Collect()_ 调用时，垃圾回收器会进行清理。

内存以堆块形式分配。如果不能使数据放入分配的块，则可以分配更多内存。在应用程序关闭之前，堆块将保留在 Mono 中。换句话说，Mono 不会释放用于操作系统的任何内存 (Unity 3.x)。一旦分配了一定数量的内存，该内存就会保留给 Mono，而不再可用于操作系统。即使释放内存后，也只能在内部用于 Mono，而不能用于操作系统。性能分析器中的堆内存值只会增加，永不减少。

如果系统无法将新数据放入已分配的堆块，则 Mono 会调用“GC”并可分配新的堆块（例如，由于碎片化）。

“太多堆区段”意味着已经耗尽了 Mono 内存（因为碎片化或大量使用）。

使用 `System.GC.GetTotalMemory` 可获取使用的 Mono 内存总量。

一般的建议是，使用尽可能小的分配。

###Unity 内存

Unity 内存用于存储资源数据（纹理、网格、音频、动画等）、游戏对象、引擎内部数据（渲染、粒子、物理等）。
使用 `Profiler.usedHeapSize` 可获取使用的 Unity 内存总量。

###内存映射
尚无工具，但可使用以下工具。


* Unity Profiler - 不完美，略过某些东西，但可获得概况。适用于设备！
* 内部性能分析器。显示已使用的堆和已分配的堆 - 请参阅 Mono 内存。显示每帧的 Mono 分配数。
* Xcode 工具 - iOS
* Xcode Instruments Activity Monitor - Real Memory 列。
* Xcode Instruments Allocations - 已创建的对象和活动状态对象的净分配。
* VM Tracker（通常使用 IOKit 标签分配纹理，而网格通常进入 VM Allocate）。

您也可以使用 Unity API 调用来创建自己的工具：


* `FindObjectsOfTypeAll (type : Type) : Object[]`
* `FindObjectsOfType (type : Type): Object[]`
* `GetRuntimeMemorySize (o : Object) : int	`
* `GetMonoHeapSize`
* `GetMonoUsedSize`
* `Profiler.BeginSample/EndSample` - 对您自己的代码进行性能分析
* `UnloadUnusedAssets () : AsyncOperation`
* `System.GC.GetTotalMemory/Profiler.usedHeapSize`

对已加载对象的引用 - 没有办法计算此数据。一种变通方法是针对公共变量“查找场景中的引用”。

###垃圾回收器


* 当系统无法将新数据放入已分配的堆块时，将触发此回收器。
* 不要在移动端使用 `OnGUI()`：它会每帧发出几次，完全重绘视图，并生成大量需要调用垃圾收集的内存分配调用。

###太快创建/删除太多对象？

* 这可能会导致碎片化。
* 使用 Editor 性能分析器跟踪内存活动。
* 可使用内部性能分析器跟踪 Mono 内存活动。
* `System.GC.Collect()`：允许暂时性中断时，可使用此 _.Net_ 函数。


###分配暂时性中断

* 使用预分配的可重用类实例的列表来实现您自己的内存管理方案。
* 不要为每个帧进行大量分配，而应进行缓存和预分配
* 碎片化问题？

###预分配内存池。

* 保留非活动游戏对象的列表并重用这些对象，而不是进行对象实例化和销毁。

###Mono 内存不足

* 对内存活动进行性能分析 - 第一个内存页面什么时候填满？
* 真的需要这么多的游戏对象，单个内存页面还不够吗？
* 对本地数据使用结构而不是类。类存储在堆上；结构存储在栈上。
* 阅读[了解自动内存管理](UnderstandingAutomaticMemoryManagement.html)页面。

###内存不足崩溃

在某些时候，游戏可能会因“内存不足”而崩溃，但理论上内存是足够的。发生这种情况时，请比较正常游戏内存占用量和崩溃发生时分配的内存大小。如果数字相差较大，则表示存在内存峰值。这可能是由于：

* 正在同时加载两个大场景 - 在两个较大的场景之间使用一个空场景来解决此问题。
* 附加场景加载 - 删除未使用的部分以保持内存大小。
* 巨大的资源包加载到内存中
* 未经正确压缩的纹理（移动端不允许此情况）。
* 启用了 Get/Set 像素的纹理。这种情况下需要在内存中保留纹理的未压缩副本。
* 在运行时从 JPEG/PNG 加载的纹理基本上是未压缩的。
* 大型 mp3 文件在加载时标记为解压缩。
* 将未使用的资源保存在奇怪的缓存中，例如静态 monobehavior 字段，这些字段在更改场景时不会被清除。
