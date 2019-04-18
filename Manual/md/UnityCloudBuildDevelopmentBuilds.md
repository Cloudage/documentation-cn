# 开发版

默认情况下，Unity Cloud Build 会编译项目以用于发布。要创建开发版，请选择编译目标的 __Advanced Options__。（请参阅有关访问和编辑 [Advanced Options](UnityCloudBuildAdvancedOptions.html) 的文档。）



![Edit Advanced Options 屏幕](../uploads/Main/UnityCloudBuildAdvancedOptions-AdvancedOptionsEdit.png)

选中 __Development Builds__ 复选框。启用后，这会在 `BuildOptions` API 中设置两个标志：

*`Development`：开发版包含调试符号并启用性能分析器 (Profiler)。

* `AllowDebugging`：允许脚本调试器远程连接到播放器。

请注意，要对 Web 播放器版本进行性能分析，您需要安装 Web 播放器的开发版本。只能在 Web 播放器开发版处于活动状态时对 .unity3d 文件进行性能分析。请参阅有关 [Profiler 窗口](Profiler.html)的文档以了解关于如何对开发版进行性能分析的更多信息。
