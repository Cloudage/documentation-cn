启动时运行 Editor 脚本代码
====================================


有时，若能够在 Unity 启动时立即在项目中运行一些 Editor 脚本代码而无需用户进行操作，这会很有用处。为实现此目的，可将 __InitializeOnLoad__ 属性应用于具有**静态构造函数**的类。静态构造函数是一个与类同名的函数，声明为 static，没有返回类型或参数（有关更多信息，请参阅[此处](http://docs.go-mono.com/index.aspx?link=ecmaspec%3a17.11)）：



````
using UnityEngine;
using UnityEditor;

[InitializeOnLoad]
public class Startup {
    static Startup()
    {
        Debug.Log("Up and running");
    }
}

````

在使用任何静态函数或类实例之前始终会调用静态构造函数，但 InitializeOnLoad 属性可确保在 Editor 启动时调用该函数。

如何使用这种技术的一个例子是在 Editor 中设置常规回调（在某种程度上相当于“帧更新”）。EditorApplication 类有一个名为 [update](../ScriptReference/EditorApplication-update.html) 的委托会在 Editor 运行时每秒调用很多次。要在项目启动时启用此委托，可使用如下代码：



````
using UnityEditor;
using UnityEngine;

[InitializeOnLoad]
class MyClass
{
    static MyClass ()
    {
        EditorApplication.update += Update;
    }

    static void Update ()
    {
        Debug.Log("Updating");
    }
}

````
