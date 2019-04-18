#反照率颜色和透明度

![此处的标准着色器材质采用了默认参数并且未分配任何值或纹理。Albedo（反照率）颜色参数已突出显示。](../uploads/Main/StandardShaderParameterAlbedoColor.png)

Albedo 参数控制着表面的基色。

![一组从黑色到白色的反照率值](../uploads/Main/StandardShaderAlbedoGraduationTable.svg)

为 Albedo 值指定单一颜色有时很有用，但为 Albedo 参数指定纹理贴图的做法更为常见。纹理贴图应表示对象表面的颜色。必须注意的是，反照率纹理**不**应包含任何光照，因为光照将根据看到对象的上下文添加到纹理中。

![两个典型反照率纹理贴图示例。左边是角色模型的纹理贴图，右边是木箱。请注意它们没有阴影和光照亮点。](../uploads/Main/StandardShaderAlbedoTextureExamples.jpg)

##透明度
反照率颜色的 Alpha 值控制着材质的透明度级别。仅当材质的 Rendering Mode（渲染模式）设置为 **Opaque** 之外的 Transparent 模式之一时，此设置才有效。如上所述，选择正确的透明度模式非常重要，因为此模式可确定您是否仍然会看到处于全值状态的反射和镜面高光，或它们是否也会根据透明度值淡出。

![从 0 到 1 范围内的透明度值，采用适合于逼真透明对象的 Transparent 模式](../uploads/Main/StandardShaderTransparencyGraduationTable.jpg)

使用为 Albedo 参数指定的纹理时，可通过确保反照率纹理图像具有 **Alpha 通道**来控制材质的透明度。Alpha 通道值映射到透明度级别，其中以白色表示完全不透明，黑色表示完全透明。这将使材质可具有透明度不同的区域。

![带 RGB 通道和 Alpha 通道的导入纹理。可单击 RGB/A 按钮（如图所示）来切换所预览的图像的通道。](../uploads/Main/StandardShaderTransparencyMapRGBAlphaToggle.jpg)

![最终结果是透过破碎的窗户窥视建筑物内部。玻璃的缺口位置是完全透明的，玻璃碎片是部分透明的，而框架是完全不透明的。](../uploads/Main/StandardShaderTransparencyMapBrokenWindow.jpg)
