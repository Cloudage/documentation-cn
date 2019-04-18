# 自定义脚本 #define 指令

使用 Unity Cloud Build 可以创建自定义脚本 #define 指令。
在 [Unity 开发者网站](https://developer.cloud.unity3d.com)上，转至编译目标的 __Advanced Options__。（请参阅有关访问和编辑 [Advanced Options](UnityCloudBuildAdvancedOptions.html) 的文档。）



![Edit Advanced Options 屏幕](../uploads/Main/UnityCloudBuildAdvancedOptions-AdvancedOptionsEdit.png)

在 __Scripting Define Symbols__ 字段中，您可以将自己的自定义脚本 #define 指令添加到可用的内置选项中。针对每个编译目标，输入要定义的符号的名称。随后可以将这些符号用作 #if 指令中的条件，就像内置条件一样。

有关更多信息，请参阅有关[依赖于平台的编译 (Platform-dependent compilation)](PlatformDependentCompilation.html) 的文档。
