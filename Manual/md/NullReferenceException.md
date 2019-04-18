# 什么是 Null 引用异常 (Null Reference Exception)？

尝试访问未引用任何对象的引用变量时，便发生 `NullReferenceException`。如果引用变量未引用任何对象，则将其视为 `null`。当变量为 `null` 时，运行时将通过发出 `NullReferenceException` 来告知正在尝试访问对象。

c# 和 JavaScript 中的引用变量在概念上类似于 C 和 C++ 中的指针。引用类型默认为 `null`，表示未引用任何对象。因此，如果尝试访问被引用的对象而又没有该对象，则将出现 `NullReferenceException`。

当代码中出现 `NullReferenceException` 时，意味着在使用变量之前忘记设置该变量。错误消息将如下所示：

    NullReferenceException: Object reference not set to an instance of an object
      at Example.Start () [0x0000b] in /Unity/projects/nre/Assets/Example.cs:10

此错误消息表明 `NullReferenceException` 发生在脚本文件 `Example.cs` 的第 10 行。此外，该消息还指出异常发生在 `Start()` 函数内。根据这些信息，比较容易查找和修复 Null 引用异常。在此示例中，代码为：

    //c# 示例
    using UnityEngine;
    using System.Collections;

    public class Example : MonoBehaviour {

    	// 使用此函数进行初始化
        void Start () {
    	    GameObject go = GameObject.Find("wibble");
    		Debug.Log(go.name);
    	}

    }

该代码简单地查找一个名为“wibble”的游戏对象。在此示例中不存在该名称的游戏对象，因此 `Find()` 函数返回 `null`。在下一行（第 9 行），我们使用 `go` 变量并尝试打印出其引用的游戏对象的名称。因为我们正在访问一个不存在的游戏对象，所以运行时抛出一个 `NullReferenceException`

Null 检查
-----------
虽然发生这种情况时令人沮丧，但这只是意味着需要更加注意脚本。这个简单示例中的解决方案是按如下所示的方式更改代码：

    using UnityEngine;
    using System.Collections;

    public class Example : MonoBehaviour {

    	void Start () {
        	GameObject go = GameObject.Find("wibble");
    	    if (go) {
    		    Debug.Log(go.name);
    		} else {
    			Debug.Log("No game object called wibble found");
    		}
    	}
	
    }

现在，在我们尝试对 `go` 变量执行任何操作之前，我们检查它是不是 `null`。如果是 `null`，则显示一条消息。

Try/Catch 代码块
----------------
出现 `NullReferenceException` 的另一个原因是使用了应该在 Inspector 中初始化的变量。如果忘记这样做，则变量将为 `null`。处理 `NullReferenceException` 的另一种方法是使用 try/catch 代码块。例如，以下代码：


    using UnityEngine;
    using System;
    using System.Collections;

    public class Example2 : MonoBehaviour {

    	public Light myLight; // 在 Inspector 中设置

        void Start () {
    	    try {
    		    myLight.color = Color.yellow;
    		}		
    		catch (NullReferenceException ex) {
        		Debug.Log("myLight was not set in the inspector");
    	    }
        }

    }

在此代码示例中，名为 `myLight` 的变量是应该在 Inspector 窗口中设置的 `Light`。如果未设置此变量，则默认为 `null`。尝试在 `try` 代码块中改变光源的颜色会导致被 `catch` 代码块捕获到的 `NullReferenceException`。`catch` 代码块将显示一条消息，此消息可能对美术师和游戏设计师更有帮助，并提醒他们在 Inspector 中设置光源。

总结
-------

* 脚本代码尝试使用未设置（引用）的变量和对象时，便会发生 `NullReferenceException`。
* 出现的错误消息可显示有关在代码中何处发生了问题的大量信息。
* 通过编写代码在访问对象之前检查 `null` 或使用 try/catch 代码块，可避免 `NullReferenceException`。
