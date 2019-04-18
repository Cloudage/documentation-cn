#按钮 (Button)

__按钮__控件可响应用户的点击并用于启动或确认操作。熟悉的示例包括 Web 表单上使用的 _Submit_ 和 _Cancel_ 按钮。

![按钮。](../uploads/Main/UI_ButtonExample.png)

![](../uploads/Main/UI_ButtonInspector.png) 

##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Interactable__ | 此组件是否接受输入？请参阅 [Interactable](script-Selectable.html)。 |
|__Transition__ | 确定控件以何种方式对用户操作进行可视化响应的属性。请参阅[过渡选项](script-SelectableTransition.html)。 |
|__Navigation__ | 确定控件顺序的属性。请参阅[导航选项](script-SelectableNavigation.html)。|

##事件

|**_属性：_** |**_功能：_** |
|:---|:---|
|__On Click__ | 用户单击按钮再松开时调用的 [UnityEvent](UnityEvents.html)。 |


##详细信息

按钮用于在用户单击再松开时启动某项操作。如果在松开单击之前将鼠标移开按钮控件，则不会执行操作。

按钮有一个名为 _On Click_ 的事件，当用户完成单击时会响应。典型用例包括：

* 确认某项决定（例如，开始游戏或保存游戏）
* 移动到 GUI 中的子菜单
* 取消正在进行的操作（例如，下载新场景）

