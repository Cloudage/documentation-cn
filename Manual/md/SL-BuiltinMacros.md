# 预定义的着色器预处理器宏


Unity 在编译[着色器程序](SL-ShaderPrograms.html)时会定义几个预处理器宏。

## 目标平台

|__宏：__|__目标平台：__|
|:---|:---|
|`SHADER_API_D3D11` | Direct3D 11 |
|`SHADER_API_GLCORE` | 桌面端 OpenGL“核心”(GL 3/4) |
|`SHADER_API_GLES` | OpenGL ES 2.0 |
|`SHADER_API_GLES3` | OpenGL ES 3.0/3.1 |
|`SHADER_API_METAL` | iOS/Mac Metal |
|`SHADER_API_VULKAN` | Vulkan |
|`SHADER_API_D3D11_9X` | 适用于通用 Windows 平台的 Direct3D 11“功能级别 9.x”目标 |
|`SHADER_API_PS4` | PlayStation 4。也定义了 `SHADER_API_PSSL`。 |
|`SHADER_API_XBOXONE` | Xbox One |
|`SHADER_API_PSP2` | PlayStation Vita |


`SHADER_API_MOBILE` 是针对所有常规移动平台（GLES、GLES3、METAL 和 PSP2）定义的。

此外，当目标着色语言为 GLSL 时，还会定义 `SHADER_TARGET_GLSL`（对于 OpenGL/GLES 平台来说始终会定义）。


## 着色器目标模型

`SHADER_TARGET` 被定义为与着色器目标编译模型匹配的数值（即匹配 `#pragma target` 指令）。例如，当编译到着色器模型 3.0 时，`SHADER_TARGET` 为 `30`。您可以在着色器代码中使用此宏来进行条件检查。例如：

````
# if SHADER_TARGET < 30
    // 低于着色器模型 3.0：
    // 着色器功能非常有限，执行近似操作
# else
    // 不错的功能，执行更高级的任务
# endif
````


## Unity 版本

`UNITY_VERSION` 包含 Unity 版本的数值。例如，对于 Unity 5.0.1，`UNITY_VERSION` 为 `501`。如果您需要编写使用不同着色器内置功能的着色器，则可以将其用于版本比较。例如，`#if UNITY_VERSION >= 500` 预处理器检查仅在版本为 5.0.0 或更高时可以通过。


## 编译的着色器阶段

编译每个着色器阶段时会定义预处理器宏 `SHADER_STAGE_VERTEX`、`SHADER_STAGE_FRAGMENT`、`SHADER_STAGE_DOMAIN`、`SHADER_STAGE_HULL`、`SHADER_STAGE_GEOMETRY` 或 `SHADER_STAGE_COMPUTE`。通常，在像素着色器和计算着色器之间共享着色器代码时，这些宏非常有用，可以解决某些工作必须以略有不同的方式来完成的情况。


## 平台差异 helper

不鼓励直接使用这些平台宏，因为它们并非始终有助于代码的未来验证。例如，如果您正在编写一个检查 D3D11 的着色器，您可能希望确保在将来将这项检查扩展为包含 Vulkan。应改用 Unity 定义的几个 helper 宏（在 [`HLSLSupport.cginc`](SL-BuiltinIncludes.html) 中）：

|__宏：__|__用途：__|
|:---|:---|
|`UNITY_BRANCH` | 在条件语句之前添加此宏，告知编译器应将其编译为实际分支。在 HLSL 平台上扩展为 `[branch]`。 |
|`UNITY_FLATTEN` | 在条件语句之前添加此宏，告知编译器应该将其展平以避免实际的分支指令。在 HLSL 平台上扩展为 `[flatten]`。 |
|`UNITY_NO_SCREENSPACE_SHADOWS` | 在不使用级联屏幕空间阴影贴图的平台（移动平台）上定义。 |
|`UNITY_NO_LINEAR_COLORSPACE` | 在不支持线性颜色空间的平台（移动平台）上定义。 |
|`UNITY_NO_RGBM` | 在不使用光照贴图 RGBM 压缩的平台（移动平台）上定义。 |
|`UNITY_NO_DXT5nm` | 在不使用 DXT5nm 法线贴图压缩的平台（移动平台）上定义。 |
|`UNITY_FRAMEBUFFER_FETCH_AVAILABLE` | 在可使用“帧缓冲颜色提取”功能的平台（通常为 iOS 平台 - OpenGL ES 2.0、3.0 和 Metal）上定义。 |
|`UNITY_USE_RGBA_FOR_POINT_SHADOWS` | 在点光源阴影贴图使用具有编码深度的 RGBA 纹理的平台（其他平台使用单通道浮点纹理）上定义。 |
|`UNITY_ATTEN_CHANNEL` | 定义光源衰减纹理的哪个通道包含数据；用于每像素光照代码。定义为“r”或“a”。 |
|`UNITY_HALF_TEXEL_OFFSET` | 在将纹理像素映射到像素时需要进行半纹素偏移调整的平台（例如 Direct3D 9）上定义。 |
|`UNITY_UV_STARTS_AT_TOP` | 始终定义值为 1 或 0。值为 1 表示在平台上的纹理之上的纹理 V 坐标为 0。Direct3D 类平台使用值 1；OpenGL 类平台使用值 0。 |
|`UNITY_MIGHT_NOT_HAVE_DEPTH_Texture` | 如果平台可以通过手动将深度渲染到纹理中来模拟阴影贴图或深度纹理，则定义此宏。 |
|`UNITY_PROJ_COORD(a)` | 给定一个 4 分量矢量，此宏返回一个适合投影纹理读取的纹理坐标。在大多数平台上，它直接返回给定值。 |
|`UNITY_NEAR_CLIP_VALUE` | 定义为近裁剪面的值。Direct3D 类平台使用 0.0，而 OpenGL 类平台使用 -1.0。 |
|`UNITY_VPOS_TYPE` | 定义像素位置输入 (VPOS) 所需的数据类型：D3D9 上为 `float2`，其他为 `float4`。 |
|`UNITY_CAN_COMPILE_TESSELLATION` | 在着色器编译器“理解”曲面细分着色器 HLSL 语法时定义（当前仅限 D3D11）。 |
|`UNITY_INITIALIZE_OUTPUT(type,name)` | 将给定_类型_的变量_名称_初始化为零。 |
|`UNITY_COMPILER_HLSL`, `UNITY_COMPILER_HLSL2GLSL`, `UNITY_COMPILER_CG` | 指示正在使用哪个着色器编译器来编译着色器 - 分别为：Microsoft 的 HLSL、HLSL 到 GLSL 转换器和 NVIDIA 的 Cg。有关更多详细信息，请参阅[着色语言](SL-ShadingLanguage.html)相关文档。如果您遇到编译器之间有非常具体的着色器语法处理差异，并希望为每个编译器编写不同的代码，请使用此宏。 |

* `UNITY_REVERSED_Z` - 在使用反转 Z 缓冲区的平台上定义。存储的 Z 值的范围是 1 到 0，而不是 0 到 1。


## 阴影贴图宏


根据平台的不同，声明和采样阴影贴图可能会有很大差异。Unity 有几个宏可帮助解决这个问题：

|__宏：__ |__用途：__ |
|:---|:---|
| `UNITY_DECLARE_SHADOWMAP(tex)` | 声明一个名为“tex”的阴影贴图纹理变量。|
| `UNITY_SAMPLE_SHADOW(tex,uv)` | 在给定的“uv”坐标处采样阴影贴图纹理“tex”（XY 分量是纹理位置，Z 分量是要比较的深度）。返回单个浮点值，阴影项的范围在 0 到 1 之间。|
| ` UNITY_SAMPLE_SHADOW_PROJ(tex,uv)` | 与上面类似，但是会读取投影阴影贴图。“uv”是一个 float4，所有其他分量除以 .w 来执行查找。|

__注意：__并非所有显卡都支持阴影贴图。请使用 [SystemInfo.SupportsRenderTextureFormat](../ScriptReference/SystemInfo.SupportsRenderTextureFormat.html) 检查是否支持。

## 常量缓冲区宏


Direct3D 11 将所有着色器变量分组为“常量缓冲区”。Unity 的大多数内置变量已经分组，但对于您自己的着色器中的变量，更加理想的做法是，根据预期的更新频率将它们放入单独的常量缓冲区。

对此，请使用 `CBUFFER_START(name)` 和 `CBUFFER_END` 宏：

````
CBUFFER_START(MyRarelyUpdatedVariables)
    float4 _SomeGlobalValue;
CBUFFER_END
````


## 纹理/采样器声明宏

通常，在着色器代码中使用 `texture2D` 来声明纹理和采样器对。
但是在某些平台（例如 DX11）上，纹理和采样器是单独的游戏对象，
并且可能的采样器最大数量非常有限。Unity 有一些宏来声明
没有采样器的纹理，并使用另一个纹理中的采样器对纹理进行采样。
如果您遇到采样器限制，并且知道几个纹理实际上可以共享同一个采样器
（采样器定义纹理过滤和包裹模式），请使用这些宏。

|__宏：__ |__用途：__ |
|:---|:---|
|`UNITY_DECLARE_TEX2D(name)` | 声明纹理和采样器对。 |
|`UNITY_DECLARE_TEX2D_NOSAMPLER(name)` | 声明不含采样器的纹理。 |
|`UNITY_DECLARE_TEX2DARRAY(name)` | 声明纹理数组采样器变量。 |
|`UNITY_SAMPLE_TEX2D(name,uv)` | 使用给定的纹理坐标从纹理和采样器对中采样。 |
|`UNITY_SAMPLE_TEX2D_SAMPLER( name,samplername,uv)` | 使用另一个纹理中的采样器 (samplername)，从纹理 (name) 中采样。 |
|`UNITY_SAMPLE_TEX2DARRAY(name,uv)` | 从具有 float3 UV 的纹理数组中采样；坐标的 z 分量是数组元素索引。 |
|`UNITY_SAMPLE_TEX2DARRAY_LOD(name,uv,lod)`| 从具有显式 Mipmap 级别的纹理数组中采样。|

有关更多信息，请参阅[采样器状态](SL-SamplerStates.html)文档。


## 表面着色器通道指示符


编译[表面着色器](SL-SurfaceShaders.html)时，表面着色器会为各种通道生成大量代码以产生光照。编译每个通道时，将定义以下宏之一：

|__宏：__ |__用途：__ |
|:---|:---|
| `UNITY_PASS_FORWARDBASE` | 前向渲染基础通道（主方向光、光照贴图和 SH）。 |
| `UNITY_PASS_FORWARDADD` | 前向渲染附加通道（每个通道一个光源）。 |
| `UNITY_PASS_DEFERRED` | 延迟着色通道（渲染 G 缓冲区）。 |
| `UNITY_PASS_SHADOWCASTER` | 阴影投射物和深度纹理渲染通道。 |
| `UNITY_PASS_PREPASSBASE` | 旧版延迟光照基础通道（渲染法线和镜面反射指数）。 |
| `UNITY_PASS_PREPASSFINAL` | 旧版延迟光照最终通道（应用光照和纹理）。 |

## 禁用自动升级

`UNITY_SHADER_NO_UPGRADE` 允许您禁止 Unity 自动升级或修改着色器文件。


## 另请参阅

* [内置着色器 include 文件](SL-BuiltinIncludes.html)
* [内置着色器变量](SL-UnityShaderVariables.html)
* [顶点和片元程序示例](SL-VertexFragmentShaderExamples.html)

---

<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
