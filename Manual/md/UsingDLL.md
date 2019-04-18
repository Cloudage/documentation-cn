#托管插件

通常，脚本作为源代码文件保存在项目中，并在每次源代码更改时由 Unity 进行编译。但是，也可以使用外部编译器将脚本编译为**动态链接库** (DLL)。然后可以将生成的 DLL 添加到项目中，并且它包含的类可以像普通脚本一样附加到对象。

在 Unity 中，使用脚本通常比使用 DLL 更容易。但是，可以访问以 DLL 形式提供的第三方 Mono 代码。在开发自己的代码时，可以将代码编译为 DLL 并将其添加到 Unity 项目，这样就能使用 Unity 不支持的编译器。有时可能还希望提供不含源代码的 Unity 代码（例如，对于 Asset Store 产品），此时 DLL 便是一种简单的方法。


##创建 DLL

为了创建 DLL，首先需要一个合适的编译器。并非所有能生成 .NET 代码的编译器都能保证与 Unity 协同工作，因此在使用编译器进行重要工作之前，建议使用一些可用的代码来测试该编译器是否兼容。如果 DLL 不包含依赖于 Unity API 的代码，则可以使用适当的编译器选项将其直接编译为 DLL。如果确实想使用 Unity API，则需要将 Unity 自己的 DLL 提供给编译器。在 Mac 上，这些都包含在应用程序包中（可以使用上下文菜单中的 Show Package Contents 命令来查看这个包的内部结构：右键单击或按住 Ctrl 键并单击 Unity 应用程序）：

Unity DLL 的路径通常是

	/Applications/Unity/Unity.app/Contents/Managed/

...并有两个名为 UnityEngine.dll 和 UnityEditor.dll 的 DLL。

在 Windows 上，可以在 Unity 应用程序附带的文件夹中找到这些 DLL。路径通常为

	C:\Program Files\Unity\Editor\Data\Managed

...对于 Mac OS，这些 DLL 的名称也是一样的。

编译 DLL 的具体选项将根据使用的编译器而有所不同。例如，Mono C# 编译器的命令行 **mcs** 在 Mac OS 上可能如下所示：

	mcs -r:/Applications/Unity/Unity.app/Contents/Managed/UnityEngine.dll -target:library ClassesForDLL.cs

这里，_-r_ 选项指定要包含在构建中的库的路径，在本例中为 UnityEngine 库的路径。_-target_ 选项指定需要的构建类型；“library”一词用于选择 DLL 构建。最后，要编译的源文件的名称是 _ClassesForDLL.cs_（假设此文件位于当前工作文件夹中，但可以根据需要使用完整路径来指定文件）。假设一切顺利，生成的 DLL 文件将很快出现在与源文件相同的文件夹中。


##使用 DLL

编译完成后，可以像任何其他资源一样将 DLL 文件直接拖入 Unity 项目。DLL 资源有一个折叠三角形，可用于显示库中的单独类。从 MonoBehaviour 派生的类可以像普通脚本一样拖到游戏对象上。非 MonoBehaviour 类可以按常规方式直接在其他脚本中使用。


![具有可见类并可折叠的 DLL](../uploads/Main/DLLScreenshot.png)


##关于 Visual Studio 的分步操作指南

本部分将介绍如何使用 Visual Studio 构建和集成简单的 DLL 示例，以及如何为 DLL 准备调试会话。


###设置项目

首先，打开 Visual Studio 并创建一个新项目。在 Visual Studio 中，应选择 __File &gt; New &gt; Project__，然后选择 __Visual C# &gt; Class Library__。

然后，需要填写新库的信息：

* **Name** 是命名空间（在此示例中使用“DLLTest”作为名称）。
* **Location** 是项目的父文件夹。
* **Solution name** 是项目的文件夹。

接下来，应该添加对 Unity DLL 的引用。在 Visual Studio 中，在 Solution Explorer 中打开 _References_ 的上下文菜单，然后选择 __Add Reference__。然后，选择选项 Browse &gt; select file__。

在此阶段，可以选择所需的 DLL 文件。在 Mac OS X 上，文件路径为：

```
Applications/Unity.app/Contents/Managed/UnityEngine.dll
```

在 Windows 中，路径为：

```
Program Files\Unity\Editor\Data\Managed\UnityEngine.dll
```

###代码

对于此示例，请在 Solution Browser 中将类重命名为 `MyUtilities`，并将其代码替换为以下代码：


````
using System;	
using UnityEngine;

namespace DLLTest {

	public class MyUtilities {
	
	    public int c;

	    public void AddValues(int a, int b) {
		    c = a + b;	
	    }
	
	    public static int GenerateRandom(int min, int max) {
	    	System.Random rand = new System.Random();
		    return rand.Next(min, max);
    	}
	}
}
````

代码准备好后，请构建项目以便生成 DLL 文件及其调试符号。


###在 Unity 中使用 DLL

对于此示例，请在 Unity 中创建一个新项目，并将构建的文件 &lt;项目文件夹&gt;/bin/Debug/DLLTest.dll 复制到 Assets 文件夹中。然后，在 Assets 文件夹中创建一个名为“Test”的 C# 脚本，并将其内容替换为以下代码：


````
using UnityEngine;
using System.Collections;
using DLLTest;

public class Test : MonoBehaviour {

	 void Start () {
		MyUtilities utils = new MyUtilities();
		utils.AddValues(2, 3);
		print("2 + 3 = " + utils.c);
	 }
	
	 void Update () {
		print(MyUtilities.GenerateRandom(0, 100));
	 }
}
````

将此脚本附加到场景中的对象并按 Play 时，Console 窗口中将显示 DLL 代码的输出。

###设置 DLL 的调试会话

首先，应为 DLL 准备调试符号。在 Visual Studio 中，在命令提示符下执行命令，将 &lt;项目文件夹&gt;\bin\Debug\DLLTest.pdb 作为参数传递：

```
Program Files\Unity\Editor\Data\Mono\lib\mono\2.0\pdb2mdb.exe
```

然后，将转换后的文件 _&lt;项目文件夹&gt;\bin\Debug\DLLTest.dll.mdb_ 复制到 _Assets/Plugins_ 中。

完成此设置后，可以按常规方式在 Unity 中调试使用 DLL 的代码。有关调试的更多信息，请参阅[脚本工具](ScriptingTools.html)部分。

###编译“不安全”的 C# 代码

Unity 允许编译[“不安全”的 C# 代码](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/unsafe)，但需要启用该功能。要执行此操作，请选择 __Edit__ > __Project Settings__ > __Player__，然后展开 __Other Settings__ 选项卡以显示 __Allow ‘unsafe’ Code__ 复选框。


---
* <span class="page-edit">2018-03-20  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">从 2018.1 开始，MonoDevelop 由 Visual Studio 取代</span>
* <span class="page-history">在 2018.1 版中添加了__“不安全的 C# 代码”__复选框</span>
