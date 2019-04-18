#WebGL 开发入门

##Unity WebGL 是什么？

WebGL 构建选项允许 Unity 将内容发布为 JavaScript 程序，而这些程序可使用 HTML5 技术和 WebGL 呈现 API 在 Web 浏览器中运行 Unity 内容。要为 WebGL 构建和测试内容，请在 __Build Player__ 窗口中选择 WebGL 构建目标，然后单击 __Build & Run__。

##技术概述

要在 WebGL 中运行，所有代码都需要是 JavaScript。我们使用 emscripten 编译器工具链将 Unity 运行时代码（用 C 和 C++ 编写）交叉编译成 asm.js JavaScript。asm.js 是一个非常易于优化的 JavaScript 子集，允许 JavaScript 引擎将 asm.js 代码提前 (AOT) 编译成非常高效的本机代码。

要将 .NET 游戏代码（C# 和 UnityScript 脚本）转换为 JavaScript，我们使用一种称为 [IL2CPP](IL2CPP.html) 的技术。IL2CPP 接受 .NET 字节码并将其转换为相应的 C++ 源文件，然后使用 emscripten 进行编译以将脚本转换为 JavaScript。

##平台支持

桌面平台的大多数主要浏览器的当前版本都支持 Unity WebGL 内容，但不同浏览器提供的[支持程度存在差异](webgl-browsercompatibility.html)。Unity WebGL 不支持移动设备。

并非所有 Unity 功能都可用于 WebGL 构建，主要是由于平台的限制。具体而言：

* 由于 JavaScript 中缺少线程支持，因此不支持线程。在 Unity 内部使用线程来加速性能以及在脚本代码和托管的 dll 中使用线程都是不受支持的。实质上不支持 `System.Threading` 命名空间中的任何内容。

* 无法在 Visual Studio 中调试 WebGL 构建。请参阅：[对 WebGL 构建进行调试和故障排除](webgl-debugging.html)。

* 出于安全考虑，浏览器不允许直接访问 IP 套接字来进行联网。请参阅：[WebGL 网络](webgl-networking.html)。

* WebGL 图形 API 等效于 OpenGL ES 2.0 和 3.0，存在一些限制。请参阅：[WebGL 图形](webgl-graphics.html)。

* WebGL 构建使用基于 __Web 音频__ API 的自定义音频后端。此情况下仅支持基本音频功能。请参阅：[使用 WebGL 中的音频](webgl-audio.html)。

* WebGL 是一个 AOT 平台，因此不允许使用 `System.Reflection.Emit` 来动态生成代码。在所有其他 IL2CPP 平台、iOS 和大多数游戏主机上都是如此。

---
* <span class="page-edit">2018-03-19  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">从 2018.1 开始，MonoDevelop 由 Visual Studio 取代</span>
