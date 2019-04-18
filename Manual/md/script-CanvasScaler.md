# 画布缩放器 (Canvas Scaler)

画布缩放器组件用于控制画布中 UI 元素的整体缩放和像素密度。此缩放会影响画布下的所有内容，包括字体大小和图像边框。

![](../uploads/Main/UI_CanvasScalerInspector.png) 

##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__UI Scale Mode__ |确定画布中的 UI 元素的缩放方式。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Constant Pixel Size__ |无论屏幕大小如何，UI 元素都保持相同的像素大小。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Scale With Screen Size__ |屏幕越大，UI 元素越大。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Constant Physical Size__ |无论屏幕大小和分辨率如何，UI 元素都保持相同的物理大小。 |

Constant Pixel Size 的设置：

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Scale Factor__ |按此系数缩放画布中的所有 UI 元素。 |
|__Reference Pixels Per Unit__ |如果精灵具有此“Pixels Per Unit”设置，则精灵中的每个像素将覆盖 UI 中的一个单位。 |

Scale With Screen Size 的设置：

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Reference Resolution__ |UI 布局设计的目标分辨率。如果屏幕分辨率较大，则 UI 会放大，如果较小，则 UI 会缩小。 |
|__Screen Match Mode__ |在当前分辨率的宽高比不适应参考分辨率时，用于缩放画布区域的模式。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Match Width or Height__ |以宽度、高度或二者的某种平均值作为参考来缩放画布区域。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Expand__ |水平或垂直扩展画布区域，使画布不会小于参考。 |
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Shrink__ |水平或垂直裁剪画布区域，使画布不会大于参考。 |
|__Match__ |确定是否以宽度、高度或二者的某种平均值作为参考进行缩放。 |
|__Reference Pixels Per Unit__ |如果精灵具有此“Pixels Per Unit”设置，则精灵中的每个像素将覆盖 UI 中的一个单位。 |

Constant Physical Size 的设置：

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Physical Unit__ |用于指定位置和大小的物理单位。 |
|__Fallback Screen DPI__ |在屏幕 DPI 未知时采用的 DPI。 |
|__Default Sprite DPI__ |用于精灵的每英寸像素，使其“Pixels Per Unit”设置与“Reference Pixels Per Unit”设置匹配。 |
|__Reference Pixels Per Unit__ |如果精灵具有此“Pixels Per Unit”设置，则其 DPI 将与“Default Sprite DPI”设置匹配。 |

World Space Canvas 的设置（画布 (Canvas) 组件设置为 World Space 时显示）：

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Dynamic Pixels Per Unit__ |用于 UI 中动态创建的位图（如文本）的每单位像素量。 |
|__Reference Pixels Per Unit__ |如果精灵具有此“Pixels Per Unit”设置，则精灵中的每个像素将覆盖世界中的一个单位。如果“Reference Pixels Per Unit”设置为 1，则精灵中的“Pixels Per Unit”设置将按原样使用。 |

##详细信息

对于设置为“Screen Space - Overlay”或“Screen Space - Camera”的画布，画布缩放器 UI Scale Mode 可以设置为 Constant Pixel Size、Scale With Screen Size 或 Constant Physical Size。

### Constant Pixel Size
使用 Constant Pixel Size 模式时，可在屏幕上按像素指定 UI 元素的位置和大小。这也是画布在未附加任何画布缩放器时的默认功能。但是，借助画布缩放器中的“Scale Factor”设置，可以向画布中的所有 UI 元素应用常量缩放。

### Scale With Screen Size
使用 Scale With Screen Size 模式时，可以根据指定参考分辨率的像素来指定位置和大小。如果当前屏幕分辨率大于参考分辨率，则画布会保持只具有参考分辨率的分辨率，但是会放大以便适应屏幕。如果当前屏幕分辨率小于参考分辨率，则画布会相应缩小以适应屏幕。

如果当前屏幕分辨率的宽高比与参考分辨率不同，则单独缩放每个轴以适应屏幕会形成非一致缩放，通常不希望发生这种情况。相反，ReferenceResolution 组件会使画布分辨率偏离参考分辨率，以便遵循屏幕的宽高比。可以使用 Screen Match Mode 设置控制此偏离的行为方式。

### Constant Physical Size
使用 Constant Physical Size 模式时，可按物理单位（如毫米、点或派卡）指定 UI 元素的位置和大小。此模式要求设备正确报告其屏幕 DPI。对于不报告 DPI 的设备，可以指定回退 DPI。

### World Space
对于设置为“World Space”的画布，可以使用画布缩放器来控制画布中 UI 元素的像素密度。

##提示
* 请参阅[设计用于多种分辨率的 UI](HOWTO-UIMultiResolution.html) 页面，其中逐步说明了如何结合使用矩形变换锚定和画布缩放器来创建适应不同分辨率和宽高比的 UI 布局。
