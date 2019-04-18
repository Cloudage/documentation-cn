#图像 (Image)

__图像__控件向用户显示非交互式图像。此图像可用于装饰、图标等，也可以从脚本更改图像以便反映其他控件的更改。该控件类似于[原始图像 (Raw Image)](script-RawImage.html) 控件，但为动画化图像和准确填充控件矩形提供了更多选项。但是，图像控件要求其纹理为[精灵](class-TextureImporter.html)，而原始图像可以接受任何纹理。

![图像控件](../uploads/Main/ImageCtrlExample.png)

##属性

![](../uploads/Main/UI_ImageInspector.png) 

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Source Image__ | 表示要显示的图像的纹理（必须作为[精灵](class-TextureImporter.html)导入）。 |
|__Color__ | 要应用于图像的颜色。 |
|__Material__ | 用于渲染图像的[材质](class-Material.html)。 |
|__Raycast Target__ | 是否应将此图像视为射线投射目标？ |
|__Preserve Aspect__ | 确保图像保持现有尺寸。  |
|__Set Native Size__ | 使用此按钮可将图像框的尺寸设置为纹理的原始像素大小。 |


##详细信息

必须将要显示的图像作为[精灵](class-TextureImporter.html)导入才能用于图像控件。
