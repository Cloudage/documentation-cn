#文本 (Text)

__文本__控件向用户显示非交互式文本。此控件可用于为其他 GUI 控件提供标题或标签，或显示说明或其他文本。

##属性

![](../uploads/Main/UI_TextInspector.png) 

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Text__ |控件显示的文本。 |
| **Character** |
| __Font__ | 用于显示文本的[字体](class-Font.html)。 |
|__Font Style__ | 应用于文本的样式。选项包括 _Normal_、_Bold_、_Italic_ 和 _Bold And Italic_。 |
|__Font Size__ | 显示的文本的大小。 |
|__Line Spacing__ | 文本行之间的垂直间距。 |
|__Rich Text__ | 文本中的标记元素是否应解释为[富文本](StyledText.html)样式？ |
|**Paragraph**|
|__Alignment__ | 文本的水平和垂直对齐方式。 |
|__Align by Geometry__ | 使用字形几何形状的范围（而不是字形指标）执行水平对齐。 |
|__Horizontal Overflow__ | 用于处理文本太宽而无法放入矩形内的情况的方法。提供的选项为 _Wrap_ 和 _Overflow_。 |
|__Vertical Overflow__ | 用于处理换行文本太高而无法放入矩形内的情况的方法。提供的选项为 _Truncate_ 和 _Overflow_。 |
|__Best Fit__ | Unity 应该忽略大小属性并尝试直接将文本放入控件的矩形？ |
| | |
|__Color__ | 用于渲染文本的颜色。 |
|__Material__ | 用于渲染文本的[材质](class-Material.html)。 |

默认文本元素如下所示：

![文本元素。](../uploads/Main/UI_TextExample.png)

##详细信息

有些控件（如[按钮](script-Button.html)和[开关](script-Toggle.html)）内置了文本描述。对于没有隐式文本的控件（如[滑动条](script-Slider.html)），可使用通过文本控件创建的标签来指示用途。文本对于列出说明、故事文本、对话和法律免责声明也很有用。

文本控件提供字体大小、样式以及文本对齐方式等常用参数。启用 _Rich Text_ 选项后，文本中的标记元素将视为样式信息，因此可仅让单个单词或短段使用粗体或不同颜色，等等（请参阅有关[富文本](StyledText.html)的页面以获取有关标记方案的详细信息）。


##提示

* 请参阅[效果](comp-UIEffects.html)页面以了解如何将简单阴影或轮廓效果应用于文本。

 
