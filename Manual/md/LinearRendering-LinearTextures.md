# 使用线性纹理

当纹理处于伽马颜色空间时，sRGB 采样允许 Unity Editor 在线性颜色空间中渲染着色器。当您选择使用线性颜色空间时，Editor 默认使用 sRGB 采样。如果[纹理](Textures.html)处于线性颜色空间，则需要使用线性颜色空间并为每个纹理禁用 sRGB 采样。要了解如何执行此操作，请参阅以下的[禁用 sRGB 采样](#DisablingsRGBSampling)。

如需进一步阅读这方面的信息，请参阅以下相关文档：

* [线性渲染概述](LinearLighting.html)，介绍线性和伽马颜色空间的背景信息。
* [线性或伽马工作流程](LinearRendering-LinearOrGammaWorkflow.html)，介绍应该选用线性颜色空间还是伽马颜色空间。
* [采用线性渲染的伽马纹理](LinearRendering-GammaTextures.html)，介绍线性工作流程中的伽马纹理。

## 旧版 GUI

对[旧版 GUI 系统](http://docs.unity3d.com/Manual/GUIScriptingGuide.html)元素的渲染始终在伽马空间中完成。这意味着，对于旧版 GUI 系统，__Texture Type__ 设置为 __Editor GUI and Legacy GUI__ 的纹理在导入时不会移除其伽马校正。

## 线性创作的纹理

同样重要的是，如果查找纹理、遮罩和其他纹理存在有特殊意义且未应用伽马校正的 RGB 值，则必须绕过 sRGB 采样。这样可以防止被采样纹理的值在用于着色器之前删除不存在的伽马校正，确保使用磁盘上存储的原始值进行计算。Unity 假设 GUI 纹理和法线贴图纹理都是在线性空间中创作的。

<a name="DisablingsRGBSampling"> </a> 
## 禁用 sRGB 采样

为确保将纹理作为线性颜色空间图像导入，请在纹理的 [Inspector 窗口](UsingTheInspector.html)中执行以下操作：

* 为纹理的预期用途选择适当的 __Texture Type__。

* 取消选中 __sRGB (Color Texture)__（如果显示了此选项）。

![纹理的 Inspector 窗口。注意已取消选中 __sRGB (Color Texture)__ 的设置。这样做可确保将纹理作为线性颜色空间图像导入](../uploads/Main/LinearRendering-SRGBSetting.png)
