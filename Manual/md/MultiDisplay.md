# 多显示

多显示功能允许同时在最多 8 个不同的监视器上显示应用程序的最多 8 个不同摄像机视图。此功能可用于 PC 游戏、街机游戏机和简单公共显示装置。

多显示仅在独立平台模式下运行，且在 Windows、Mac OS X 和 Linux 上均受支持。


##在项目中进行多显示预览

要查看不同的监视器显示画面，请执行以下操作：

在每个 __Camera__ 的 __Inspector__ 中设置为显示特定监视器。
然后，可通过 __Target Display__ 选项分配 1 到 8 之间的监视器（请参阅*图1*).

![图 1：包含 Target Display 选项的 Camera Inspector](../uploads/Main/InspectorCamera35.png)

然后，可在 Game 视图中使用视图左上角的 __Display__ 下拉菜单预览每个显示画面（请参阅*图2*).

![图 2：在 Game 视图左上角的显示预览](../uploads/Main/TargetDisplay.png)
 

##激活多显示
默认显示为一个监视器，因此在运行应用程序时，需要使用 __Display.Activate__ 通过脚本显式激活任何其他显示。必须显式激活每个额外的显示，并且一旦激活，就无法停用它们。

激活额外显示的最佳时间是在创建新场景时。执行此操作的一种好方法是将脚本组件附加到默认的摄像机。
确保仅在启动期间调用一次 __Display.Activate__。您可能会发现创建一个小的初始场景来进行测试会很有帮助。

**示例脚本**

```
using UnityEngine;
using System.Collections;

public class DisplayScript : MonoBehaviour
{
	// 使用此函数进行初始化
	void Start()
	{
    	Debug.Log("displays connected: " + Display.displays.Length);
    	// Display.displays[0] 是主要的默认显示，并始终为 ON。
    	// 检查是否有其他可用的显示，并激活每个显示。
    	if (Display.displays.Length > 1)
        	Display.displays[1].Activate();
    	if (Display.displays.Length > 2)
        	Display.displays[2].Activate();
    	...
	}
	//每帧调用一次 Update
	void Update()
	{

	}
}

```

##API 支持
支持以下 __UnityEngine.Display__ API 函数：

```
	public void Activate() 
```
此函数将根据当前监视器的宽度和高度激活具体显示。必须在启动新场景时进行一次此调用。
可从新场景中的__摄像机__或虚拟__游戏对象__附加的用户脚本进行此调用。

```
public void Activate(int width, int height, int refreshRate) 
```
仅限 Windows：此函数将根据自定义监视器的宽度和高度激活具体显示。


##控制监视器显示位置
默认情况下，用户的计算机会根据其 X、Y 虚拟桌面对其监视器的相对位置进行排序。要覆盖此设置以便应用程序显示时不进行任何排序，请从命令行启动应用程序并使用以下命令行标志：

`-multidisplay`


