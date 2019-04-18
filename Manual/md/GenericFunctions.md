通用函数
=================


脚本参考中列出的某些函数（例如，各种 GetComponent 函数）附带有一个变体，
该变体在函数名称后的尖括号中具有字母 T 或类型名称：


````
//C#
void FuncName<T>();
//JS
function FuncName.<T>(): T;
````
这些函数称为通用函数。这些函数对脚本的重要性在于，
可以在调用函数时指定参数类型和/或返回类型。在 JavaScript 中，
这种做法可以规避动态输入的限制：


````
// 可正确推断出类型，因为已在函数调用中定义该类型。
//在 C# 中
var obj = GetComponent<Rigidbody>();
//在 JS 中
var obj = GetComponent.<Rigidbody>();
````
在 C# 中，这样可以节省很多击键和转换：

````
Rigidbody rb = go.GetComponent<Rigidbody>();

// ...相较于：

Rigidbody rb = (Rigidbody) go.GetComponent(typeof(Rigidbody));
````
在脚本参考页面上列出了通用变体的所有函数都会允许这种
特殊调用语法。
