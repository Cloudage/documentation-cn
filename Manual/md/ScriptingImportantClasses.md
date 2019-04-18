
重要的类
=============================

以下是在使用 Unity 编写脚本时将要使用的一些最重要的类。这些类涵盖 Unity 脚本化系统的一些核心领域，为查找可用的函数和事件提供了良好的起点。


|**_类：_** |**_描述：_** |
|:---|:---|
|[MonoBehaviour](../ScriptReference/MonoBehaviour.html) |所有新 Unity 脚本的基类，MonoBehaviour 引用提供了附加到游戏对象的标准脚本可以使用的所有函数和事件的列表。如果需要对游戏中的各个对象进行任何类型的交互或控制，请使用此类。 |
|[Transform](../ScriptReference/Transform.html) |每个游戏对象都在空间（无论是 3D 还是 2D 空间）中具有位置、旋转和缩放，而这些信息由变换组件表示。除了提供这些信息之外，变换组件还有许多有用的函数可用于移动、缩放、旋转、重新父子化和操纵对象，并可将坐标从一个空间转换为另一个空间。 |
|[Rigidbody](../ScriptReference/Rigidbody.html) / [Rigidbody2D](../ScriptReference/Rigidbody2D.html) |对于大多数游戏元素，物理引擎提供了最简单的工具集来移动对象、检测触发器和碰撞以及施加作用力。Rigidbody 类（或其 2D 等效类 Rigidbody2D）提供了设定速度、质量、阻力、力、扭矩、碰撞等所需的所有属性和函数。 |

