#WebGL 性能注意事项

##可在 WebGL 上期待什么样的性能？

这个问题有点难以回答，因为取决于许多因素。

通常情况下，在 GPU 方面，可期待获得接近本机应用程序的性能，因为 WebGL 图形 API 使用 GPU 进行硬件加速渲染：将 WebGL API 调用和着色器转换为操作系统图形 API（通常为 Windows 上的 DirectX 或者 Mac 或 Linux 上的 OpenGL）只需要少量开销。

在 CPU 方面，所有代码都将转换为 asm.js JavaScript。因此，可期待的性能情况很大程度上取决于所使用的 Web 浏览器的 JavaScript 引擎，目前在这方面存在着一些非常显著的差异。在撰写本文时（2015 年 11 月），Microsoft Edge 和 Mozilla Firefox 在 Unity 代码方面可提供最佳性能，因为目前只有这些浏览器利用 asm.js 规范针对这种情况使用优化的 JavaScript 代码 AOT 编译路径；在许多编程基准方面与本机代码相比，性能下降幅度在二分之一以内，这一系数也与我们部署到 WebGL 并在 Firefox 和 Edge 中运行的不同 Unity 内容中看到的结果相符。

但是，还有其他一些考虑因素。目前，JavaScript 语言既不支持多线程也不支持 SIMD。因此，受益于这些功能的代码将比其他代码出现更大的减速。无法在脚本中为 WebGL 编写线程代码和 SIMD 代码，但有些引擎部分通常为多线程或经过 SIMD 优化，所以在 WebGL 上会因此降低运行性能。一个示例是蒙皮代码，这种代码既为多线程，又经过 SIMD 优化。借助 Unity 中新增的时间轴[性能分析器](Profiler.html)，可查看 Unity 如何将工作分配到非 WebGL 平台上的不同线程。从长远来看，我们希望这些功能也可以在 WebGL 上使用。


##影响性能的 WebGL 特有设置

为获得最佳性能，请在 Build Player 对话框中将优化级别设置为“Fastest”，并在 WebGL 的播放器设置中将“Exception support”设置为“None”。


##WebGL 性能分析

WebGL 支持 Unity Profiler，请参阅[此处](Profiler.html)了解如何对其进行设置。


##后台标签中的 WebGL 内容

如果在 [WebGL Player Settings](class-PlayerSettingsWebGL.html) 中启用了 _Run in background_，或者如果启用了 __[Application.runInBackground](../ScriptReference/Application-runInBackground.html)__，则您的内容将在画布或浏览器窗口失去焦点时继续运行。

但应该注意的是，浏览器可能会限制在后台标签中运行的内容。如果包含该内容的标签不可见，大多数浏览器中只会每秒更新一次内容。请注意，这种情况将导致 __[Time.time](../ScriptReference/Time-time.html)__ 比平常采用默认设置时更慢，因为 __[Time.maximumDeltaTime](../ScriptReference/Time-maximumDeltaTime.html)__ 的默认值小于一秒。

##限制 WebGL 性能

在某些情况下，可能希望以较低的帧率运行 WebGL 内容，以降低 CPU 使用率。与其他平台一样，可使用 [Application.targetFrameRate](../ScriptReference/Application-targetFrameRate.html) API 来实现此目的。

如果不想限制性能，请将此 API 设置为默认值 -1，而不是较高的值。如此，浏览器便可调整帧率以便在浏览器的渲染循环中实现最平滑的动画，获得的效果可能好于 Unity 尝试执行自身的主循环时序来匹配目标帧率。
