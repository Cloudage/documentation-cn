内置着色器变量
=========================


Unity 为着色器提供了一些内置的全局变量：当前对象的变换矩阵、光源参数、当前时间等等。就像任何其他变量一样，可在[着色器程序](SL-ShaderPrograms.html)中使用这些变量，唯一的区别是不必声明它们，因为它们都是在自动包含的 `UnityShaderVariables.cginc` include 文件中声明的。


## 变换

所有这些矩阵都是 `float4x4` 类型。

|                           |           |
|:---                       |:---       |
| **名称**                  | **值** |
| UNITY\_MATRIX\_MVP        | 当前模型 \* 视图 \* 投影矩阵。 |
| UNITY\_MATRIX\_MV         | 当前模型 \* 视图矩阵。 |
| UNITY\_MATRIX\_V          | 当前视图矩阵。 |
| UNITY\_MATRIX\_P          | 当前投影矩阵。 |
| UNITY\_MATRIX\_VP         | 当前视图 \* 投影矩阵。 |
| UNITY\_MATRIX\_T\_MV      | 模型转置 \* 视图矩阵。 |
| UNITY\_MATRIX\_IT\_MV     | 模型逆转置 \* 视图矩阵。 |
| unity_ObjectToWorld       | 当前模型矩阵。 |
| unity_WorldToObject       | 当前世界矩阵的逆矩阵。 |



## 摄像机和屏幕

这些变量将对应于正在渲染的[摄像机](class-Camera.html)。例如，在阴影贴图渲染中，
它们仍将引用摄像机组件值，而不是用于阴影贴图投影的“虚拟摄像机”。

|                           |           |           |
|:---                       |:---       |:---       |
| **名称**                  | **类型**  | **值** |
|\_WorldSpaceCameraPos      |float3     | 摄像机的世界空间位置。 |
|\_ProjectionParams         |float4     | `x` 是 1.0（如果当前使用[翻转投影矩阵](SL-PlatformDifferences.html)进行渲染，则为 -1.0），`y` 是摄像机的近平面，`z` 是摄像机的远平面，`w` 是远平面的倒数。 |
|\_ScreenParams             |float4     | `x` 是摄像机目标纹理的宽度（以像素为单位），`y` 是摄像机目标纹理的高度（以像素为单位），`z` 是 1.0 + 1.0/宽度，`w` 为 1.0 + 1.0/高度。 |
|\_ZBufferParams            |float4     | 用于线性化 Z 缓冲区值。`x` 是 (1-远/近)，`y` 是 (远/近)，`z` 是 (x/远)，`w` 是 (y/远)。 |
| unity_OrthoParams         |float4     | `x` 是正交摄像机的宽度，`y` 是正交摄像机的高度，`z` 未使用，`w` 在摄像机为正交模式时是 1.0，而在摄像机为透视模式时是 0.0。 |
| unity_CameraProjection    |float4x4   | 摄像机的投影矩阵。 |
| unity_CameraInvProjection |float4x4   | 摄像机投影矩阵的逆矩阵。 |
| unity_CameraWorldClipPlanes[6] |float4  | 摄像机视锥体平面世界空间方程，按以下顺序：左、右、底、顶、近、远。 |


## 时间

|                 |          |           |
|:---             |:---      |:---       |
| **名称**        | **类型** | **值** |
|_Time            |float4    | 自关卡加载以来的时间 (t/20, t, t\*2, t\*3)，用于将着色器中的内容动画化。 |
|\_SinTime        |float4    | 时间正弦：(t/8, t/4, t/2, t)。 |
|\_CosTime        |float4    | 时间余弦：(t/8, t/4, t/2, t)。 |
|unity\_DeltaTime |float4    | 增量时间：(dt, 1/dt, smoothDt, 1/smoothDt)。 |


## 光照

光源参数以不同的方式传递给着色器，具体取决于使用哪个[渲染路径](RenderingPaths.html)，
以及着色器中使用哪种光源模式[通道标签](SL-PassTags.html)。

[前向渲染](RenderTech-ForwardRendering.html)（`ForwardBase` 和 `ForwardAdd` 通道类型）：

|                                              |          |             |
|:---                                          |:---      |:---         |
| **名称**                                     | **类型** | **值**   |
|\_LightColor0*（在 Lighting.cginc 中声明）*   | fixed4   |光源颜色。 |
|\_WorldSpaceLightPos0                          | float4   |方向光：（世界空间方向，0）。其他光源：（世界空间位置，1）。 |
|\_LightMatrix0*（在 AutoLight.cginc 中声明）* | float4x4 |世界/光源矩阵。用于对剪影和衰减纹理进行采样。 |
|unity_4LightPosX0、unity_4LightPosY0、unity_4LightPosZ0 | float4 | *（仅限 ForwardBase 通道）*前四个非重要点光源的世界空间位置。 |
|unity_4LightAtten0                            | float4   | *（仅限 ForwardBase 通道）*前四个非重要点光源的衰减因子。 |
|unity_LightColor                              | half4[4] | *（仅限 ForwardBase 通道）*前四个非重要点光源的颜色。 |
|unity_WorldToShadow                            | float4x4[4]  |世界/阴影矩阵。聚光灯的一个矩阵，方向光级联最多有四个矩阵。 |


延迟着色和延迟光照，在光照通道着色器中使用（全部在 UnityDeferredLibrary.cginc 中声明）：

|                           |          |           |
|:---                       |:---      |:---       |
| **名称**                  | **类型** | **值** |
|\_LightColor                | float4 | 光源颜色。 |
|\_LightMatrix0              | float4x4 |世界/光源矩阵。用于对剪影和衰减纹理进行采样。 |
|unity_WorldToShadow                            | float4x4[4]  |世界/阴影矩阵。聚光灯的一个矩阵，方向光级联最多有四个矩阵。 |


为 `ForwardBase`、`PrePassFinal` 和 `Deferred` 通道类型设置了球谐函数系数
（由环境光和光照探针使用）。这些系数包含由世界空间法线求值的三阶 SH 函数（请参阅 [UnityCG.cginc](SL-BuiltinIncludes.html) 中的 `ShadeSH9`）。
这些变量都是 half4 类型、`unity_SHAr` 和类似名称。


[顶点光照渲染](RenderTech-VertexLit.html)（`Vertex` 通道类型）：

最多可为 `Vertex` 通道类型设置 8 个光源；始终从最亮的光源开始排序。因此，如果您希望
一次渲染受两个光源影响的对象，可直接采用数组中前两个条目。如果影响对象
的光源数量少于 8，则其余光源的颜色将设置为黑色。

|                            |           |             |
|:---                        |:---       |:---         |
| **名称**                   | **类型**  | **值**   |
|unity_LightColor            | half4[8]  | 光源颜色。 |
|unity_LightPosition         | float4[8] | 视图空间光源位置。方向光为 (-direction,0)；点光源/聚光灯为 (position,1)。 |
|unity_LightAtten            | half4[8]  | 光源衰减因子。_x_ 是 cos(spotAngle/2) 或 -1（非聚光灯）；_y_ 是1/cos(spotAngle/4) 或 1（非聚光灯）；_z_ 是二次衰减；_w_ 是平方光源范围。 |
|unity_SpotDirection         | float4[8] | 视图空间聚光灯位置；非聚光灯为 (0,0,1,0)。 |


## 雾效和环境光

|                           |          |           |
|:---                       |:---      |:---       |
| **名称**                  | **类型** | **值** |
|unity_AmbientSky           |fixed4    | 梯度环境光照情况下的天空环境光照颜色。 |
|unity_AmbientEquator        |fixed4    | 梯度环境光照情况下的赤道环境光照颜色。 |
|unity_AmbientGround        |fixed4    | 梯度环境光照情况下的地面环境光照颜色。 |
|UNITY_LIGHTMODEL_AMBIENT   |fixed4    | 环境光照颜色（梯度环境情况下的天空颜色）。旧版变量。 |
|unity_FogColor             |fixed4    | 雾效颜色。 |
|unity_FogParams            |float4    | 用于雾效计算的参数：(density / sqrt(ln(2))、density / ln(2)、-1/(end-start) 和 end/(end-start))。_x_ 对于 Exp2 雾模式很有用；_y_ 对于 Exp 模式很有用，_z_ 和 _w_ 对于 Linear 模式很有用。 |


##其他

|               |          |           |
|:---           |:---      |:---       |
| **名称**      | **类型** | **值** |
|unity_LODFade  |float4    | 使用 [LODGroup](class-LODGroup.html) 时的细节级别淡入淡出。_x_ 为淡入淡出（0 到 1），_y_ 为量化为 16 级的淡入淡出，_z_ 和 _w_ 未使用。 |
|_TextureSampleAdd|float4|Set automatically by Unity **for UI only** based on whether the texture being used is in Alpha8 format (the value is set to (1,1,1,0)) or not (the value is set to (0,0,0,0)).|
