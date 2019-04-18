## 编写后期处理效果

[后期处理](PostProcessingOverview.html)是一种在 Unity 中将效果应用于渲染图像的方法。

任何使用 [OnRenderImage](../ScriptReference/Camera.OnRenderImage.html) 函数的 Unity [脚本](CreatingAndUsingScripts.html)都可以用作后期处理效果。必须将其添加到[摄像机](class-Camera.html)[游戏对象](GameObjects.html)才能使脚本执行后期处理。

### OnRenderImage 函数

[OnRenderImage](../ScriptReference/Camera.OnRenderImage.html) Unity Scripting API 函数接受两个参数：

* 作为 [RenderTexture](class-RenderTexture.html) 的源图像

* 应该渲染到的目标（也是 RenderTexture）。

后期处理效果通常使用[着色器](class-Shader.html)。这些着色器将读取源图像，对其进行一些计算，并将结果渲染到目标中（例如，使用 [Graphics.Blit](../ScriptReference/Graphics.Blit.html)）。后期处理效果将完全替代目标的所有像素。

摄像机可以有多个后期处理效果，每个效果都作为[组件](UsingComponents.html)。Unity 按照这些效果在 [Inspector](UsingTheInspector.html) 中列出的顺序以堆栈形式执行它们；位于 Inspector 顶部的后期处理组件先渲染。在这种情况下，第一个后期处理组件的结果作为“源图像”传递给下一个后期处理组件。在内部，Unity 会创建一个或多个临时渲染纹理以保存这些中间结果。

请注意，后期处理栈中的后期处理组件列表未指定它们的应用顺序。

注意事项：

* 目标渲染纹理可以是 null，这意味着“渲染到屏幕”（即后缓冲区）。摄像机上的最后一个后期处理效果将发生此情况。

* `OnRenderImage` 完成后，Unity 期望目标渲染纹理是激活的渲染目标。通常情况下，[Graphics.Blit](../ScriptReference/Graphics.Blit.html) 或手动渲染到目标纹理应该是最后的渲染操作。

* 在后期处理效果着色器中关闭深度缓冲区写入和测试。这样可确保 [Graphics.Blit](../ScriptReference/Graphics.Blit.html) 不会将非预期值写入目标 Z 缓冲区。几乎所有后期处理着色器 pass 都应包含 `Cull Off ZWrite Off ZTest Always` 状态。

* 要使用原始场景渲染中的模板或深度缓冲区值，请使用 [Graphics.SetRenderTarget](../ScriptReference/Graphics.SetRenderTarget.html) 将原始场景渲染中的深度缓冲区显式绑定为深度目标。应传递第一个源图像效果深度缓冲区作为要绑定的深度缓冲区。

### 渲染不透明对象之后执行后期处理效果

默认情况下，Unity 在渲染整个场景后执行后期处理效果。在某些情况下，您可能更希望 Unity 在渲染场景中的所有不透明对象之后但在渲染其他对象之前渲染后期处理效果（例如，在渲染[天空盒](class-Skybox.html)或[透明对象](StandardShaderMaterialParameterAlbedoColor.html)之前）。像景深 (Depth of Field) 这样基于深度的效果经常使用这种方式。

为此，请在 [OnRenderImage](../ScriptReference/Camera.OnRenderImage.html) Unity Scripting API 函数中添加 [ImageEffectOpaque](../ScriptReference/ImageEffectOpaque.html) 属性。

### 不同平台上的纹理坐标

如果后期处理效果一次采样多个不同的屏幕相关纹理，您可能需要了解不同平台如何使用纹理坐标。一种常见的情况是效果“源”纹理和摄像机的[深度纹理](SL-CameraDepthTexture.html)需要不同的垂直坐标，具体取决于抗锯齿设置。有关更多信息，请参阅 Unity 用户手册[平台差异](SL-PlatformDifferences.html)页面。

### 相关主题

* [深度纹理](SL-CameraDepthTexture.html)通常用于图像后期处理，以获得屏幕上每个像素距最近不透明表面的距离。

* 对于 [HDR](HDR.html) 渲染，[ImageEffectTransformsToLDR](../ScriptReference/ImageEffectTransformsToLDR.html) 属性指示使用色调映射。

* 还可以使用[命令缓冲区](GraphicsCommandBuffers.html)来执行后期处理。

* 使用 [RenderTexture.GetTemporary](../ScriptReference/RenderTexture.GetTemporary.html) 来获取临时渲染纹理并在后期处理效果中进行计算。

* 另请参阅 Unity 用户手册的[编写着色器程序](SL-ShaderPrograms.html)页面。

---

* <span class="page-edit"> 2017-05-24  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.6 中的新功能</span>
