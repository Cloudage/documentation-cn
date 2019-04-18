#渲染模式 (Rendering Mode)

![此处的标准着色器材质采用了默认参数并且未分配任何值或纹理。Rendering Mode 参数已突出显示。](../uploads/Main/StandardShaderParameterRenderMode.png)

标准着色器中的第一个材质参数为 **Rendering Mode**。此参数允许您选择对象是否使用透明度，如果是，使用哪种类型的混合模式。

- **Opaque** - 此项为默认设置，适用于没有透明区域的普通固体对象。

- **Cutout** - 用于创建在不透明区域和透明区域之间具有硬边的透明效果。在这种模式下，没有半透明区域，纹理为 100% 不透明或不可见。使用透明度来创建材质的形状时（如树叶或者有孔洞和碎布条的布料），这非常有用。

- **Transparent** - 适用于渲染逼真的透明材质，如透明塑料或玻璃。在此模式下，材质本身将采用透明度值（基于纹理的 Alpha 通道和色调颜色的 Alpha），但与真实透明材质的情况一样，反射和光照高光将保持完全清晰可见。

- **Fade** - 允许透明度值完全淡出对象，包括对象可能具有的任何镜面高光或反射。如果要对淡入或淡出的对象进行动画化，此模式将非常有用。它不适合渲染逼真的透明材质，如透明塑料或玻璃，因为反射和高光也会淡出。

![此图像中的头盔罩使用 Transparent 模式渲染而成，因为它应该表示具有透明属性的真实物理对象。此处的头盔罩正在反射场景中的天空盒。](../uploads/Main/StandardShaderTransparencySkyBoxReflection.jpg) 

![这些窗户使用了 Transparent 模式，但在纹理中定义了一些完全不透明的区域（窗框）。来自光源的镜面反射将反射透明区域和不透明区域。](../uploads/Main/StandardShaderTransparentWindow.jpg)

![此图像中的全息图使用 Fade 模式渲染而成，因为它应该表示部分淡出的不透明对象。](../uploads/Main/StandardShaderFadeHologram.jpg)

![此图像中的草使用 Cutout 模式渲染而成。此模式为对象提供了清晰的锐利边缘（通过指定截止阈值进行定义）。Alpha 值高于此阈值的图像的所有部分都是 100% 不透明的，而低于此阈值的所有部分都是不可见的。在图像的右侧，可看到材质设置和所用纹理的 Alpha 通道。](../uploads/Main/StandardShaderCutoutGrassExample.jpg)
