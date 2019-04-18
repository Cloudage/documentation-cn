# 内容大小适配器 (Content Size Fitter)

##属性

![](../uploads/Main/UI_ContentSizeFitterInspector.png) 

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Horizontal Fit__ |如何控制宽度。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Unconstrained |不根据布局元素伸展宽度。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Min Size |根据布局元素的最小宽度来伸展宽度。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Preferred Size |根据布局元素的偏好宽度来伸展宽度。 |
|__Vertical Fit__ |如何控制高度。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Unconstrained |不根据布局元素伸展高度。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Min Size |根据布局元素的最小高度来伸展高度。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Preferred Size |根据布局元素的偏好高度来伸展高度。 |


##描述

内容大小适配器充当布局控制器，可用于控制其自身布局元素的大小。大小由游戏对象上布局元素组件提供的最小大小或偏好大小确定。此类布局元素可以是图像或文本组件、布局组或布局元素组件。

值得注意的是，当调整矩形变换的大小时（无论是通过内容大小适配器还是其他工具），大小调整是围绕轴心进行的。这意味着可使用轴心来控制大小调整的方向。

例如，当轴心位于中心位置时，内容大小适配器将在所有方向朝外均匀扩展矩形变换。当轴心位于左上角时，内容大小适配器将向右下方向扩展矩形变换。
