# 导航选项

![](../uploads/Main/UI_SelectableNavigation.png) 

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Navigation__ | 导航选项表示如何控制 UI 元素在播放模式中的导航。 |
|__None__ | 无键盘导航。还可以确保单击/点击操作不会获得焦点。  |
|__Horizontal__ | 水平导航。 |
|__Vertical__ | 垂直导航。 |
|__Automatic__ | 自动导航。 |
|__Explicit__ | 在此模式下，可显式指定不同箭头键将控件导航到的位置。 |
|__Visualize__ | 选择 Visualize 可以直观显示您在场景窗口中设置的导航。请参阅下文。 |

![](../uploads/Main/UI_SelectableNavigationExplicit.png) 

![场景窗口显示了可视化导航连接](../uploads/Main/GUIVisualizeNavigation.png)

在以上可视化模式中，箭头指示如何为整个控件集合设置焦点变化。这意味着，对于每个单独的 UI 控件，如果用户在给定控件具有焦点时按下箭头键，您可以看到接下来将获得焦点的 UI 控件。因此，在上面显示的示例中，如果“Button”具有焦点并且用户按下向右箭头键，则第一个（左侧）垂直滑动条随后将获得焦点。请注意，无法使用向上或向下键将焦点从垂直滑动条上移开，因为这两个键用于控制滑动条的值。水平滑动条和左/右箭头键也是同样道理。
