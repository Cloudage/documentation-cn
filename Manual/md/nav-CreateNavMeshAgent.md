#创建导航网格代理

为关卡烘焙导航网格后，即可创建能够在场景中导航的角色了。我们将使用圆柱体构建原型代理，并将代理设置为运动状态。为实现此目的，需要使用导航网格代理 (NavMesh Agent) 组件和简单脚本。

![](../uploads/Main/NavMeshAgentSetup.svg) 

首先，让我们创建角色：

1.创建一个**圆柱体**：__GameObject &gt; 3D Object &gt; Cylinder__。
2.默认的圆柱体尺寸（高度 2 和半径 0.5）适用于人形代理，因此我们将这些尺寸保持原样。
3.添加一个**导航网格代理**组件：__Component &gt; Navigation &gt; NavMesh Agent__。

现在已设置简单的导航网格代理来准备接收命令！

开始尝试使用导航网格代理时，很可能需要根据角色大小和速度调整代理的尺寸。

__导航网格代理__组件将负责角色的寻路和移动控制。在脚本中，导航的设置十分简单，只需设置所需的目标点：导航网格代理可从此处进行所有内容的处理。

````
	// MoveTo.cs
		using UnityEngine;
		using System.Collections;
	
		public class MoveTo : MonoBehaviour {
	   
		   public Transform goal;
	   
		   void Start () {
			  NavMeshAgent agent = GetComponent<NavMeshAgent>();
			  agent.destination = goal.position; 
		   }
		} 
````

接下来，我们需要构建一个简单的脚本，通过此脚本可将角色发送到另一个游戏对象指定的目标，并构建一个用作移动目标的球体：

1.创建一个新的 **C# 脚本** (``MoveTo.cs``) 并将其内容替换为以上脚本。
2.将 MoveTo 脚本分配给刚刚创建的角色。
3.创建一个**球体**，此球体将用作代理移动到的目标。
4.将球体从角色移动到靠近导航网格表面的位置。
5.选择角色，找到 MoveTo 脚本，并将球体分配给 **Goal** 属性。
6.**按 Play**；您应该会看到代理导航到球体的位置。

总而言之，在脚本中，您需要获取对导航网格代理组件的引用，然后为了将代理设置为运动状态，只需将一个位置分配给其 [destination](../ScriptReference/AI.NavMeshAgent-destination.html) 属性。[导航操作方法](nav-HowTos.html)部分提供了进一步的一些示例来说明如何使用导航网格代理解决常见的游戏问题。


###阅读更多信息

- [导航操作方法](nav-HowTos.html) - 导航网格代理的常见用例以及源代码。
- [导航系统的内部工作原理](nav-InnerWorkings.html) - 了解有关寻路的更多信息。
- [导航网格代理组件参考](class-NavMeshAgent.html) – 所有导航网格代理属性的完整描述。
- [导航网格代理脚本参考](../ScriptReference/AI.NavMeshAgent.html) - NavMeshAgent 脚本 API 的完整描述。
