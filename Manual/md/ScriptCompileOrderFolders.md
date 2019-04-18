#特殊文件夹和脚本编译顺序

Unity 保留了一些项目文件夹名称来指示内容具有特殊用途。其中一些文件夹会影响脚本编译的顺序。这些文件夹名称为：

* Assets
* Editor
* Editor default resources
* Gizmos
* Plugins
* Resources
* Standard Assets
* StreamingAssets

请参阅[特殊文件夹名称](SpecialFolders.html)了解这些文件夹的用途。

脚本编译过程有四个独立的阶段。编译脚本的阶段由其父文件夹确定。

在脚本必须引用其他脚本中定义的类的情况下，这很重要。基本规则是无法引用在当前阶段*之后*的阶段编译的任何内容。在当前阶段或早期阶段编译的所有内容则是完全可用的。

当用一种语言编写的脚本必须引用另一种语言定义的类（例如，UnityScript 文件声明 C# 脚本中定义的类的变量）时，会出现另一种情况。这里的规则是被引用的类必须在早期阶段完成编译。

编译阶段如下：

* **第 1 阶段：**名为 __Standard Assets__、__Pro Standard Assets__ 和 __Plugins__ 的文件夹中的运行时脚本。
* **第 2 阶段：**名为 __Editor__ 的文件夹（位于名为 __Standard Assets__、__Pro Standard Assets__ 和 __Plugins__ 的顶层文件夹中的任意位置）中的 Editor 脚本。
* **第 3 阶段：**不在名为 __Editor__ 的文件夹中的所有其他脚本。
* **第 4 阶段：**其余所有脚本（位于名为 __Editor__ 的文件夹中的脚本）。


体现此顺序非常重要的一个常见示例是 UnityScript 文件需要引用 C# 文件中定义的类时。要实现此引用，必须将 C# 文件放在 Plugins 文件夹中，并将 UnityScript 文件放在非特殊文件夹中。如果不这样做，则会抛出错误，指出无法找到 C# 类。

**注意**：Standard Assets 仅在 __Assets__ 根文件夹中有效。
