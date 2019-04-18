# 内置着色器 helper 函数

Unity 具有许多内置实用函数，旨在使编写着色器更简单，更轻松。

### UnityCG.cginc 中声明的函数

请参阅[内置着色器 include 文件](SL-BuiltinIncludes.html)来大致了解 Unity 随附的着色器 include 文件。

#### UnityCG.cginc 中的顶点变换函数

|**功能：** |**描述：** |
|:---|:---|
|`float4 UnityObjectToClipPos(float3 pos)`|将对象空间中的点变换到齐次坐标中的摄像机裁剪空间。这等效于 __mul(UNITY_MATRIX_MVP, float4(pos, 1.0))__，应该在适当的位置使用。|
|`float3 UnityObjectToViewPos(float3 pos)`|将对象空间中的点变换到视图空间。这等效于 __mul(UNITY_MATRIX_MV, float4(pos, 1.0)).xyz__，应该在适当的位置使用。|
#### UnityCG.cginc 中的通用 helper 函数

|**功能：** |**描述：** |
|:---|:---|
| `float3 WorldSpaceViewDir (float4 v)` | 返回从给定对象空间顶点位置朝向摄像机的世界空间方向（未标准化）。|
| `float3 ObjSpaceViewDir (float4 v)` | 返回从给定对象空间顶点位置朝向摄像机的对象空间方向（未标准化）。|
| `float2 ParallaxOffset (half h, half height, half3 viewDir)` | 计算视差法线贴图的 UV 偏移。|
| `fixed Luminance (fixed3 c)` | 将颜色转换为亮度（灰阶）。|
| `fixed3 DecodeLightmap (fixed4 color)` | 从 Unity 光照贴图（RGBM 或 dLDR，具体取决于平台）解码颜色。|
| `float4 EncodeFloatRGBA (float v)` | 将 [0..1) 范围浮点数编码为 RGBA 颜色，用于存储在低精度渲染目标中。|
| `float DecodeFloatRGBA (float4 enc)` | 将 RGBA 颜色解码为浮点数。|
| `float2 EncodeFloatRG (float v)` | 将 [0..1) 范围浮点数编码为 float2。 |
| `float DecodeFloatRG (float2 enc)` | 解码先前编码的 RG 浮点数。 |
| `float2 EncodeViewNormalStereo (float3 n)` | 将视图空间法线编码为 0 到 1 范围内的两个数字。|
| `float3 DecodeViewNormalStereo (float4 enc4)` | 从 enc4.xy 解码视图空间法线。|

#### UnityCG.cginc 中的前向渲染 helper 函数

仅当使用前向渲染（ForwardBase 或 ForwardAdd 通道类型）时，这些函数才有用。


|**功能：** |**描述：** |
|:---|:---|
| `float3 WorldSpaceLightDir (float4 v)` | 根据给定的对象空间顶点位置计算朝向光源的世界空间方向（未标准化）。|
| `float3 ObjSpaceLightDir (float4 v)` | 根据给定对象空间顶点位置计算朝向光源的对象空间方向（未标准化）。|
| `float3 Shade4PointLights (...)` | 计算四个点光源的光照，将光源数据紧密打包到矢量中。前向渲染使用它来计算每顶点光照。|


#### UnityCG.cginc 中的屏幕空间 helper 函数

以下 helper 函数可计算用于采样屏幕空间纹理的坐标。它们返回 `float4`，其中用于纹理采样的最终坐标可以通过透视除法（例如 `xy/w`）计算得出。

这些函数还处理渲染纹理坐标中的[平台差异](SL-PlatformDifferences.html)。

|**功能：** |**描述：** |
|:---|:---|
| `float4 ComputeScreenPos (float4 clipPos)` | 计算用于执行屏幕空间贴图纹理采样的纹理坐标。输入是裁剪空间位置。 |
| `float4 ComputeGrabScreenPos (float4 clipPos)` | 计算用于 [GrabPass](SL-GrabPass.html) 纹理采样的纹理坐标。输入是裁剪空间位置。 |



#### UnityCG.cginc 中的顶点光照 helper 函数

仅当使用每顶点光照着色器（“Vertex”通道类型）时，这些函数才有用。


|**功能：** |**描述：** |
|:---|:---|
| `float3 ShadeVertexLights (float4 vertex, float3 normal)` | 根据给定的对象空间位置和法线计算四个每顶点光源和环境光的光照。 |
