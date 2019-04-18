#平台特定的渲染差异

Unity 在如下各种图形库平台上运行：[Open GL](https://www.opengl.org)、[Direct3D](https://msdn.microsoft.com/en-us/library/windows/desktop/hh309466.aspx)、[Metal](https://developer.apple.com/metal/) 和游戏主机。在某些情况下，平台与着色器语言语义之间的图形渲染行为方式存在差异。大多数情况下，Unity Editor 会隐藏这些差异，但在某些情况下，Editor 无法为您执行此操作。在这些情况下，您需要确保消除平台之间的差异。下面列出了这些情况以及发生这些情况时需要采取的操作。

##渲染纹理坐标
垂直纹理坐标约定在两种类型的平台之间有所不同，分别是 Direct3D 类和 OpenGL 类平台。


* **Direct3D 类**：顶部坐标为 0 并向下增加。此类型适用于 Direct3D、Metal 和游戏主机。
* **OpenGL 类**：底部坐标为 0 并向上增加。此类适用于 OpenGL 和 OpenGL ES。

除了渲染到[渲染纹理](class-RenderTexture.html)的情况下，这种差异不会对您的项目产生任何影响。在 Direct3D 类平台上渲染到纹理时，Unity 会在内部上下翻转渲染。这样就会使坐标约定在平台之间匹配，并以 OpenGL 类平台约定作为标准。

在着色器中，有两种常见情况需要您采取操作确保不同的坐标约定不会在项目中产生问题，这两种情况就是图像效果和 UV 空间中的渲染。

###图像效果
使用[图像效果](PostProcessingWritingEffects.html)和抗锯齿时，系统不会翻转为图像效果生成的源纹理来匹配 OpenGL 类平台约定。在这种情况下，Unity 渲染到屏幕以获得抗锯齿效果，然后将渲染解析为渲染纹理，以便通过图像效果进行进一步处理。

如果您的图像效果是一次处理一个渲染纹理的简单图像效果，则 [Graphics.Blit](../ScriptReference/Graphics.Blit.html) 会处理不一致的坐标。但是，如果您在[图像效果](PostProcessingWritingEffects.html)中一起处理多个[渲染纹理](class-RenderTexture.html)，则在 Direct3D 类平台中以及在您使用抗锯齿时，渲染纹理很可能以不同的垂直方向出现。要标准化坐标，必须在顶点着色器中手动上下“翻转”屏幕纹理，使其与 OpenGL 类坐标标准匹配。

以下代码示例演示了如何执行此操作：

```
// 翻转纹理的采样：
// 主纹理的
// 纹理像素大小将具有负 Y。

# if UNITY_UV_STARTS_AT_TOP
if (_MainTex_TexelSize.y < 0)
        uv.y = 1-uv.y;
# endif

```

请参阅 Unity 的着色器替换示例项目中的边缘检测场景（请参阅 [Unity 学习资源](http://unity3d.com/support/resources/example-projects/shader-replacement)），了解与此相关的更详细示例。此项目中的边缘检测同时使用屏幕纹理和摄像机的[深度+法线纹理](SL-CameraDepthTexture.html)。

[GrabPass](SL-GrabPass.html) 也出现了类似的情况。生成的渲染纹理实际上可能不会在 Direct3D 类（非 OpenGL 类）平台上进行上下翻转。如果着色器代码对 GrabPass 纹理进行采样，请使用 [UnityCG include](SL-BuiltinFunctions.html) 文件中的 `ComputeGrabScreenPos` 函数。

###在 UV 空间中渲染
在纹理坐标 (UV) 空间中渲染特殊效果或工具时，您可能需要调整着色器，以便在 Direct3D 类和 OpenGL 类系统之间进行一致渲染。您还可能需要在渲染到屏幕和渲染到纹理之间进行渲染调整。为进行此类调整，应上下翻转 Direct3D 类投影，使其坐标与 OpenGL 类投影坐标相匹配。

[内置变量](SL-UnityShaderVariables.html) `ProjectionParams.x` 包含值 `+1` 或 `–1`。`-1` 表示投影已上下翻转以匹配 OpenGL 类投影坐标，而 `+1` 表示尚未翻转。
您可以在着色器中检查此值，然后执行不同的操作。下面的示例将检查是否已翻转投影，如果已翻转，则再次进行翻转，然后返回 UV 坐标以便匹配。

```
float4 vert(float2 uv : TEXCOORD0) : SV_POSITION
{
    float4 pos;
    pos.xy = uv;
    // 此示例使用上下翻转的投影进行渲染，
    // 因此也翻转垂直 UV 坐标
    if (_ProjectionParams.x < 0)
        pos.y = 1 - pos.y;
    pos.z = 0;
    pos.w = 1;
    return pos;
}
```

##裁剪空间坐标
与纹理坐标类似，裁剪空间坐标（也称为投影后空间坐标）在 Direct3D 类和 OpenGL 类平台之间有所不同：

* **Direct3D 类**：裁剪空间深度从近平面的 0.0 到远平面的 +1.0。此类型适用于 Direct3D、Metal 和游戏主机。

* **OpenGL 类**：裁剪空间深度从近平面的 –1.0 到远平面的 +1.0。此类适用于 OpenGL 和 OpenGL ES。

在着色器代码内，可使用[内置宏](SL-BuiltinMacros.html) `UNITY_NEAR_CLIP_VALUE` 来获取基于平台的近平面值。

在脚本代码内，使用 [GL.GetGPUProjectionMatrix](../ScriptReference/GL.GetGPUProjectionMatrix.html) 将 Unity 的坐标系（遵循 OpenGL 类约定）转换为 Direct3D 类坐标（如果这是平台所期望的）。

##着色器计算的精度
要避免精度问题，请确保在目标平台上测试着色器。移动设备和 PC 中的 GPU 在处理浮点类型方面有所不同。PC GPU 将所有浮点类型（浮点精度、半精度和固定精度）视为相同；PC GPU 使用完整 32 位精度进行所有计算，而许多移动设备 GPU 并不是这样做。

有关详细信息，请参阅[数据类型和精度](SL-DataTypesAndPrecision.html)的文档。

##着色器中的 const 声明
`const` 的使用在 Microsoft HSL（请参阅 <a href="http://msdn.microsoft.com">msdn.microsoft.com</a>）和 OpenGL 的 GLSL（请参阅 [Wikipedia](https://en.wikipedia.org/wiki/OpenGL_Shading_Language)）着色器语言之间有所不同。

* Microsoft 的 HLSL `const` 与 C# 和 C++ 中的含义大致相同：声明的变量在其作用域内是只读的，但可按任何方式初始化。

* OpenGL 的 GLSL `const` 表示变量实际上是编译时常量，因此必须使用编译时约束（文字值或其他对于 `const` 的计算）进行初始化。

最好是遵循 OpenGL 的 GLSL 语义，并且只有当变量真正不变时才将变量声明为 `const`。避免使用其他一些可变值初始化 `const` 变量（例如，作为函数中的局部变量）。这一原则也适用于 Microsoft 的 HLSL，因此以这种方式使用 `const` 可以避免在某些平台上混淆错误。

##着色器使用的语义
要让着色器在所有平台上运行，一些着色器值应该使用以下语义：

* __顶点着色器输出（裁剪空间）位置__：`SV_POSITION`。有时，着色器使用 POSITION 语义来使着色器在所有平台上运行。请注意，这不适用于 Sony PS4 或有曲面细分的情况。

* __片元着色器输出颜色__：`SV_Target`。有时，着色器使用 `COLOR` 或 `COLOR0` 来使着色器在所有平台上运行。请注意，这不适用于 Sony PS4。

将网格渲染为点时，从顶点着色器输出 `PSIZE` 语义（例如，将其设置为 1）。某些平台（如 OpenGL ES 或 Metal）在未从着色器写入点大小时会将点大小视为“未定义”。

有关更多详细信息，请参阅有关[着色器语义](SL-ShaderSemantics.html)的文档。

##Direct3D 着色器编译器语法
Direct3D 平台使用 Microsoft 的 [HLSL 着色器编译器](SL-ShadingLanguage.html)。对于各种细微的着色器错误，HLSL 编译器比其他编译器更严格。例如，它不接受未正确初始化的函数输出值。

使用此编译器时，您可能遇到的最常见情况是：

* 具有 `out` 参数的[表面着色器](SL-SurfaceShaders.html)顶点修改器。按如下方式初始化输出：


  ```
  void vert (inout appdata_full v, out Input o) 
      {
        **UNITY_INITIALIZE_OUTPUT(Input,o);**
        // ...
      }
  ```

* 部分初始化的值。例如，函数返回 `float4`，但代码只设置它的 `.xyz` 值。如果只需要三个值，请设置所有值或更改为 `float3`。

* 在顶点着色器中使用 `tex2D`。这是无效的，因为顶点着色器中不存在 UV 导数。这种情况下，您需要采样显式 Mip 级别；例如，使用 `tex2Dlod` (`tex, float4(uv,0,0)`)。此外，还需要添加 `#pragma target 3.0`，因为 `tex2Dlod` 是着色器模型 3.0 的功能。

##着色器中的 DirectX 11 (DX11) HLSL 语法

[表面着色器](SL-SurfaceShaders.html)编译管线的某些部分不能理解特定于 DirectX 11 的 HLSL（Microsoft 的着色器语言）语法。

如果您正在使用 HLSL 功能（比如 `StructuredBuffers`、`RWTextures` 和其他非 DirectX 9 语法），请将它们包裹在 DirectX X11 专用的预处理器宏中，如下例所示。

```
# ifdef SHADER_API_D3D11
// DirectX11 专用代码，例如
StructuredBuffer<float4> myColors;
RWTexture2D<float4> myRandomWriteTexture;
# endif
```

##使用着色器帧缓冲提取
一些 GPU（最明显的是 iOS 上基于 PowerVR 的 GPU）允许您通过提供当前片元颜色作为片元着色器的输入来进行某种可编程混合（请参阅 [khronos.org](https://www.khronos.org/registry/gles/extensions/EXT/EXT_shader_framebuffer_fetch.txt) 上的 `EXT_shader_framebuffer_fetch`）。

可在 Unity 中编写使用帧缓冲提取功能的着色器。要执行此操作，请在使用 HLSL（Microsoft 的着色语言，请参阅 <a href="http://msdn.microsoft.com">msdn.microsoft.com</a>）或 Cg（Nvidia 的着色语言，请参阅 [nvidia.co.uk](http://www.nvidia.co.uk/)）编写片元着色器时使用 `inout` 颜色参数。

以下示例采用的是 Cg 语言。

```
CGPROGRAM
// 只为可能支持该功能的平台（目前是 gles、gles3 和 metal）
// 编译着色器
# pragma only_renderers framebufferfetch

void frag (v2f i, inout half4 ocol : SV_Target)
{
    // ocol 可以被读取（当前帧缓冲区颜色）
    // 并且可以被写入（将颜色更改为该颜色）
    // ...
}   
ENDCG
```

##着色器中的深度 (Z) 方向

深度 (Z) 方向在不同的着色器平台上不同。

**DirectX 11、DirectX 12、PS4、Xbox One、Metal：反转方向**

* 深度 (Z) 缓冲区在近平面处为 1.0，在远平面处减小到 0.0。

* 裁剪空间范围是 [near,0]（表示近平面处的近平面距离，在远平面处减小到 0.0）。

**其他平台：传统方向**

* 深度 (Z) 缓冲区值在近平面处为 0.0，在远平面处为 1.0。

* 裁剪空间取决于具体平台：
    * 在 Direct3D 类平台上，范围是 [0,far]（表示在近平面处为 0.0，在远平面处增加到远平面距离）。
    * 在 OpenGL 类平台上，范围是 [-near,far]（表示在近平面处为负的近平面距离，在远平面处增加到远平面距离）。

请注意，使反转方向深度 (Z) 与浮点深度缓冲区相结合，可显著提高相对于传统方向的深度缓冲区精度。这样做的优点是降低 Z 坐标的冲突并改善阴影，特别是在使用小的近平面和大的远平面时。

因此，在使用深度 (Z) 发生反转的平台上的着色器时：

* 定义了 UNITY_REVERSED_Z。
* `_CameraDepth` 纹理的纹理范围是 1（近平面）到 0（远平面）。
* 裁剪空间范围是“near”（近平面）到 0（远平面）。

但是，以下宏和函数会自动计算出深度 (Z) 方向的任何差异：

* `Linear01Depth(float z)`
* `LinearEyeDepth(float z)`
* UNITY_CALC_FOG_FACTOR(coord)

###提取深度缓冲区
如果要手动提取深度 (Z) 缓冲区值，则可能需要检查缓冲区方向。以下是执行此操作的示例：

```
float z = tex2D(_CameraDepthTexture, uv);
# if defined(UNITY_REVERSED_Z)
    z = 1.0f - z;
# endif
```

###使用裁剪空间
如果要手动使用裁剪空间 (Z) 深度，则可能还需要使用以下宏来抽象化平台差异：

`float clipSpaceRange01 = UNITY_Z_0_FAR_FROM_CLIPSPACE(rawClipSpace);`

**注意**：此宏不会改变 OpenGL 或 OpenGL ES 平台上的裁剪空间，因此在这些平台上，此宏返回“-near”1（近平面）到 far（远平面）之间的值。

###投影矩阵
如果处于深度 (Z) 发生反转的平台上，则 [GL.GetGPUProjectionMatrix()](../ScriptReference/GL.GetGPUProjectionMatrix.html) 返回一个还原了 z 的矩阵。
但是，如果要手动从投影矩阵中进行合成（例如，对于自定义阴影或深度渲染），您需要通过脚本按需自行还原深度 (Z) 方向。

以下是执行此操作的示例：

```
var shadowProjection = Matrix4x4.Ortho(...); //阴影摄像机投影矩阵
var shadowViewMat = ...     //阴影摄像机视图矩阵
var shadowSpaceMatrix = ... //从裁剪空间到阴影贴图纹理空间
    
//当引擎通过摄像机投影计算设备投影矩阵时，
//“m_shadowCamera.projectionMatrix”被隐式反转
m_shadowCamera.projectionMatrix = shadowProjection; 

//“shadowProjection”在连接到“m_shadowMatrix”之前被手动翻转，
//因为它被视为着色器的其他矩阵。
if(SystemInfo.usesReversedZBuffer) 
{
    shadowProjection[2, 0] = -shadowProjection[2, 0];
    shadowProjection[2, 1] = -shadowProjection[2, 1];
    shadowProjection[2, 2] = -shadowProjection[2, 2];
    shadowProjection[2, 3] = -shadowProjection[2, 3];
}
    m_shadowMatrix = shadowSpaceMatrix * shadowProjection * shadowViewMat;
```

###深度 (Z) 偏差
Unity 自动处理深度 (Z) 偏差，以确保其与 Unity 的深度 (Z) 方向匹配。但是，如果要使用本机代码渲染插件，则需要在 C 或 C++ 代码中消除（反转）深度 (Z) 偏差。

####深度 (Z) 方向检查工具
* 使用 [SystemInfo.usesReversedZBuffer](../ScriptReference/SystemInfo-usesReversedZBuffer.html) 可确认所在平台是否使用反转深度 (Z)。
