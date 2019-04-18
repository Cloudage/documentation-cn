# 摄像机的深度纹理


[摄像机](class-Camera.html)可生成深度、深度+法线或运动矢量纹理。这是一种极简化的 G 缓冲纹理，可用于后期处理效果或实现自定义光照模型（例如光照预通道）。还可以使用[着色器替换](SL-ShaderReplacement.html)功能来自行构建类似纹理。

可使用脚本中的 [Camera.depthTextureMode](../ScriptReference/Camera-depthTextureMode.html) 变量来启用摄像机的深度纹理模式。

有三种可能的深度纹理模式：

* __DepthTextureMode.Depth__：[深度纹理](SL-DepthTextures.html)。
* __DepthTextureMode.DepthNormals__：打包到一个纹理中的深度和视图空间法线。*
* __DepthTextureMode.MotionVectors__：当前帧的每个屏幕纹理像素的每像素屏幕空间运动。打包到 RG16 纹理中。

这些是标志，因此可以指定上述纹理的任何组合。

## DepthTextureMode.Depth 纹理


这将构建一个屏幕大小的[深度纹理](SL-DepthTextures.html)。

渲染深度纹理时使用的着色器通道与用于阴影投射物渲染的着色器通道相同（`ShadowCaster` 通道类型）。因此，通过扩展，如果着色器不支持阴影投射（即在着色器或任何后备着色器中没有阴影投射物通道），则使用该着色器的对象将不会显示在深度纹理中。

* 让着色器[回退](SL-Fallback.html)到另一个具有阴影投射通道的着色器，或者
* 如果您在使用[表面着色器](SL-SurfaceShaders.html)，那么添加 `addshadow` 指令也能让它们生成阴影通道。

请注意，仅“不透明”对象（这些对象的材质和着色器设置为使用小于等于 2500 的[渲染队列](SL-SubShaderTags.html)）会渲染到深度纹理中。


## DepthTextureMode.DepthNormals 纹理


这将构建屏幕大小的 32 位（8 位/通道）纹理，其中视图空间法线编码到 R&G 通道中，而深度编码到 B&A 通道中。法线使用立体投影进行编码，深度是打包到两个 8 位通道中的 16 位值。

[`UnityCG.cginc` include 文件](SL-BuiltinIncludes.html)具有一个 helper 函数 `DecodeDepthNormal`，可从编码的像素值中解码深度和法线。返回的深度在 0 到 1 范围内。

有关如何使用深度和法线纹理的示例，请参阅着色器替换示例项目中的 EdgeDetection 图像效果或[屏幕空间环境光遮挡图像效果](PostProcessing-AmbientOcclusion.html)。

## DepthTextureMode.MotionVectors 纹理


这将构建屏幕大小的 RG16（16 位浮点/通道）纹理，其中屏幕空间像素运动编码到 R＆G 通道中。像素运动编码到屏幕 UV 空间中。

从该纹理运动采样时，将返回 -1 到 1 范围内的编码像素。这将是从最后一帧到当前帧的 UV 偏移。


## 提示和技巧

[摄像机检视面板](class-Camera.html)会指示摄像机何时渲染深度或深度+法线纹理。

从摄像机请求深度纹理的方式 ([Camera.depthTextureMode](../ScriptReference/Camera-depthTextureMode.html)) 可能意味着在禁用需要深度纹理的效果后，摄像机可能仍会继续渲染深度纹理。若摄像机上存在多个效果，其中每个效果都需要深度纹理，则无法在禁用单个效果的情况下自动禁用深度纹理渲染。

在实现复杂的着色器或图像效果时，切记[平台之间的渲染差异](SL-PlatformDifferences.html)。尤其是，在图像效果中使用深度纹理通常需要对 Direct3D 和抗锯齿进行特殊处理。

在某些情况下，深度纹理可能直接来自本机 Z 缓冲区。如果在深度纹理中看到了瑕疵，请确保使用该纹理的着色器**未**写入 Z 缓冲区（使用 [ZWrite Off](SL-CullAndDepth.html)）。


## 着色器变量

深度纹理可用于在着色器中作为全局着色器属性进行采样。通过声明名为 `_CameraDepthTexture` 的采样器，您将能够为摄像机采样主深度纹理。

`_CameraDepthTexture` 始终引用摄像机的主深度
纹理。相比之下，您可以使用 `_LastCameraDepthTexture` 来引用任何摄像机渲染的最后一个深度纹理。例如，如果您使用辅助摄像机渲染脚本中的半分辨率深度纹理并希望将其提供给后期处理着色器，这种做法可能很有用。

运动矢量纹理（启用时）在着色器中可用作全局着色器属性。通过声明名为“_CameraMotionVectorsTexture”的采样器，您可以为当前执行渲染的摄像机采样纹理。

## 背景

深度纹理可以直接来自实际深度缓冲区，
也可以在单独通道中渲染，具体取决于使用的渲染
路径和硬件。通常，在使用延迟
着色或旧版延迟光照渲染路径时，会自动采用深度纹理，因为它们始终都是 
G 缓冲区渲染的产物。

当 DepthNormals 纹理在单独通道中渲染时，此过程通过[着色器替换](SL-ShaderReplacement.html)完成。因此，务必在您的着色器中设置正确的“__RenderType__”标签。

启用时，MotionVectors 纹理始终来自额外的渲染通道。Unity 会将移动的游戏对象渲染到此缓冲区中，并构建从最后一帧到当前帧的运动。


## 另请参阅

* [编写图像效果](PostProcessingWritingEffects.html)
* [深度纹理](SL-DepthTextures.html)
* [着色器替换](SL-ShaderReplacement.html)


