#触摸输入模块

**注意：触摸输入模块 (TouchInputModule) 已弃用。现在触摸输入的处理在 [StandaloneInputModule](script-StandaloneInputModule.html) 中进行。**

该模块设计用于触摸设备，可发送触摸和拖动操作的指针事件来响应用户输入。该模块支持多点触控。

该模块使用场景配置的射线投射器来计算当前正在触摸的元素。每次当前触摸操作都将相应发出射线投射。
 

##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Force Module Active__ | 强制激活此模块。 |

##详细信息

该模块的流程如下：

- 对于每个触摸事件
    - 如果这是新的按压操作
        - 发送 PointerEnter 事件（向上发送到层级视图中可对其进行处理的每个对象）
        - 发送 PointerPress 事件
        - 缓存拖动处理程序（层级视图中可对其进行处理的第一个元素）
        - 将 BeginDrag 事件发送到拖动处理程序
        - 在事件系统中将“Pressed”对象设置为 Selected
    - 如果这是持续按压操作
        - 处理移动
        - 将 DragEvent 发送到缓存的拖动处理程序
        - 如果触摸在对象之间移动，则处理 PointerEnter 和 PointerExit 事件
    - 如果这是释放操作
        - 将 PointerUp 事件发送到收到 PointerPress 的对象
        - 如果当前悬停对象与 PointerPress 对象相同，则发送 PointerClick 事件
        - 如果缓存了拖动处理程序，则发送 Drop 事件
        - 将 EndDrag 事件发送到缓存的拖动处理程序
