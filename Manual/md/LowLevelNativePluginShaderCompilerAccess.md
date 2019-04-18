#低级原生插件着色器编译器访问

除了[低级原生插件接口](NativePluginInterface.html)之外，Unity 还支持对着色器编译器的低级访问，从而允许用户将不同的变体注入着色器。这也是一种事件驱动的方法：当某些内置事件发生时，插件将接收回调。

Unity 公开的着色器编译器访问扩展定义位于 IUnityShaderCompilerAccess.h 中，随 Editor 提供。

目前仅在 D3D11 上支持这些扩展。随后将有更多平台支持这些扩展。

##着色器编译器访问扩展 API

为了利用渲染扩展，插件应导出 UnityShaderCompilerExtEvent。include 文件中提供了大量文档。

只要 Unity 触发其中一个内置事件，就会通过 UnityShaderCompilerExtEvent 调用插件。还可使用脚本通过 CommandBuffer.IssuePluginEventAndData 或 CommandBuffer.IssuePluginCustomBlit 命令将回调添加到 CommandBuffer。

除了基本脚本接口之外，Unity 中的[原生代码插件](Plugins.html)还可以在发生某些事件时接收回调。这主要用于实现插件中的低级渲染，并使低级渲染能够与 Unity 的多线程渲染一起使用。

定义 Unity 所公开的接口的头文件随 Editor 一起提供。

##着色器编译器访问配置接口

Unity 提供了一个配置着色器编译器访问的接口 (IUnityShaderCompilerExtPluginConfigure)。此接口由插件用于保留自己的关键字并配置着色器程序和 GPU 程序编译器掩码（应该调用插件的着色器或 GPU 程序的类型）

---
 
* <span class="page-history">Unity [2017.1](https://docs.unity3d.com/2017.1/Documentation/Manual/30_search.html?q=newin20171) 中的新功能 <span class="search-words">NewIn20171</span></span>

* <span class="page-edit">2017-07-04 Page published with no [editorial review](DocumentationEditorialReview.html)
</span>
