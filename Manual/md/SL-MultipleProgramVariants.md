#编写多个着色器程序变体


通常方便的做法是保持大部分着色器代码固定，但也允许生成稍微不同的着色器“变体”。这种变体通常称为“大型着色器”或“超级着色器”，实现的方式是针对每种情况使用不同的预处理器指令多次编译着色器代码。

在 Unity 中，可通过向[着色器代码片段](SL-ShaderPrograms.html)中添加 `#pragma multi_compile` 或 `#pragma shader_feature` 指令来实现目的。这在[表面着色器](SL-SurfaceShaders.html)中也有效。

在运行时将从 Material 关键字（Material.EnableKeyword 和 DisableKeyword）或全局着色器关键字（Shader.EnableKeyword 和 DisableKeyword）中选取适当的着色器变体。


## multi\_compile 的工作方式

如下指令：

````
# pragma multi_compile FANCY_STUFF_OFF FANCY_STUFF_ON
````

将生成两个着色器变体，一个定义了 `FANCY_STUFF_OFF`，另一个定义了 `FANCY_STUFF_ON`。在运行时，将根据“Material”或全局着色器关键字激活其中一个变体。如果这两个关键字均未启用，则将使用第一个（“off”）。

一行 multi\_compile 中可以有两个以上的关键字，例如，以下指令将生成四个着色器变体：

````
# pragma multi_compile SIMPLE_SHADING BETTER_SHADING GOOD_SHADING BEST_SHADING
````

当任何名称为全下划线时，将生成未定义预处理器宏的着色器变体。这通常用于着色器功能，以避免使用多达两个关键字（请参阅下面的关键字限制说明）。例如，下面的指令将生成两个着色器变体；第一个变体未定义任何内容，第二个变体定义了 `FOO_ON`：

````
# pragma multi_compile __ FOO_ON
````


## shader\_feature 与 multi\_compile 之间的区别

`#pragma shader_feature` 与 `#pragma multi_compile` 非常类似，唯一的区别是 shader\_feature 着色器的未使用变体不会包含在游戏构建中。因此，shader\_feature 对于将在材质上设置的关键字最合适，而 multi\_compile 对于将通过代码进行全局设置的关键字最合适。

此外，它有一个只包含一个关键字的速记符号：

````
# pragma shader_feature FANCY_STUFF
````

这只是 `#pragma shader_feature _ FANCY_STUFF` 的快捷方式，即它会扩展为两个着色器变体（第一个没有定义；第二个有定义）。



##合并多个 multi\_compile 行

可提供多个 multi_compile 行，此情况下将针对所有可能的行组合来编译生成的着色器：

````
# pragma multi_compile A B C
# pragma multi_compile D E
````

此指令将为第一行产生三种变体，为第二行产生两种变体，或者总共产生六种着色器变体（A+D、B+D、C+D、A+E、B+E 和 C+E）。

最简单的方法是将每个 multi\_compile 行视为控制单个着色器“功能”。请记住，着色器变体的总数会以这种方式急速增长。例如，十个带有两个选项的 multi\_compile“功能”总共将生成 1024 个着色器变体！


##关键字限制

使用着色器变体时，请记住 Unity 中关键字数量上限是 256，其中大约 60 个保留供内部使用（因此降低了可用上限）。此外，关键字会在整个特定 Unity 项目中全局启用，因此在多个不同着色器中定义多个关键字时，请注意不要超过限制。


## 内置 multi\_compile 快捷方式

有几个“快捷方式”符号用于编译多个着色器变体；它们主要用于处理 Unity 中不同的光照、阴影和光照贴图类型。请参阅[渲染管线](SL-RenderPipeline.html)以了解详细信息。

* `multi_compile_fwdbase` 将编译 `ForwardBase`（前向渲染基础）通道类型需要的所有变体。这些变体处理不同的光照贴图类型以及开启或关闭阴影的主方向光。
* `multi_compile_fwdadd` 将编译 `ForwardAdd`（前向渲染附加）通道类型的变体。这将编译变体来处理方向光、聚光灯或点光源类型，以及它们带有剪影纹理的变体。
* `multi_compile_fwdadd_fullshadows` - 同上，但还能够让光源具有实时阴影。
* `multi_compile_fog` 扩展为多个变体以处理不同的雾效类型 (off/linear/exp/exp2)。


大多数内置快捷方式都会产生许多着色器变体。如果您知道不需要某些变体，可通过使用 `#pragma skip_variants` 来跳过这些变体的编译。例如：

````
# pragma multi_compile_fwdadd
// 将跳过包含
// "POINT" 或 "POINT_COOKIE" 的所有变体
# pragma skip_variants POINT POINT_COOKIE
````

## 着色器硬件变体

使用着色器变体的一个常见原因是创建可同时在单个目标平台（例如 OpenGL ES）内的高端和低端硬件上高效运行的后备着色器或简化着色器。要为不同的硬件功能级别提供特别优化的变体集，可以使用着色器硬件变体。

要启用着色器硬件变体的生成，请添加 `#pragma hardware_tier_variants renderer`，其中 `renderer` 是[着色器程序杂注](SL-ShaderPrograms.html)的可用渲染平台之一。使用此 `#pragma`，将为每个着色器生成 3 个着色器变体，并忽略其他所有关键字。每个变体将定义以下指令之一：

```
UNITY_HARDWARE_TIER1
UNITY_HARDWARE_TIER2
UNITY_HARDWARE_TIER3
```

您可以使用它们为更低端或更高端硬件编写条件性回退或额外功能。在 Editor 中，您可以使用 Graphics Emulation 菜单测试任何层（允许您在每个层之间进行更改）。

为了帮助尽可能降低这些变体的影响，播放器中只加载了一组着色器。此外，任何最终相同的着色器（例如，如果您只为 TIER1 编写专用版本而其他所有版本都相同）将不占用磁盘上的任何额外空间。

在加载时，Unity 将检查其正在使用的 GPU 并自动检测层值；如果未自动检测到 GPU，它将默认为最高层。您可以通过设置 `Shader.globalShaderHardwareTier` 来覆盖此层值，但必须在加载要更改的任何着色器之前完成此操作。加载着色器后，着色器将选择一组变体，此值将无效。一个良好的设置位置是在加载主场景之前的预加载场景中。

请注意，这些着色器硬件层与播放器的质量设置无关，它们纯粹是从运行播放器的 GPU 的相对功能中检测到的。

## 平台着色器设置

除了调整不同硬件层的着色器代码之外，您可能还需要调整 Unity 内部定义（例如，您可能希望在移动设备上强制使用级联阴影贴图）。您可以在 [UnityEditor.Rendering.PlatformShaderSettings](../ScriptReference/Rendering.PlatformShaderSettings.html) 文档中找到与此相关的详细信息，其中提供了按层覆盖的最新支持功能的列表。
使用 [UnityEditor.Rendering.EditorGraphicsSettings.SetShaderSettingsForPlatform](../ScriptReference/Rendering.EditorGraphicsSettings.SetShaderSettingsForPlatform.html) 可根据平台按层调整硬件着色器设置。

请注意，如果设置为不同层的 `PlatformShaderSetting` 不相同，则即使缺少 `#pragma hardware_tier_variants`，也会为着色器生成层变体。

### 另请参阅

* [优化着色器加载时间](OptimizingShaderLoadTime.html)。
