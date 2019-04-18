特性
==========


__特性 (Attribute)__ 是可以放在脚本中的类、属性或函数上方来指示特殊行为的标记。例如，__HideInInspector__ 特性可以添加到属性声明中，从而防止该属性（即使是公共属性）显示在检视面板中。在 JavaScript 中，特性名称以“@”符号开头，而在 C# 中，它包含在方括号内：

````
// JS

@HideInInspector
var strength: float;


// C#

[HideInInspector]
public float strength;
````

Unity 提供了若干特性；请查看脚本参考（从侧栏的弹出菜单中选择 Editor 或 Runtime Attributes 部分）了解这些属性。.NET 库中还定义了一些有时可能在 Unity 代码中很有用的特性。

**注意：**不应使用 .NET 库中定义的 [ThreadStatic](http://msdn.microsoft.com/en-us/library/system.threadstaticattribute.aspx) 特性，因为如果将其添加到 Unity 脚本，会导致崩溃。

