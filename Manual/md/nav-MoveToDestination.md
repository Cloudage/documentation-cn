#告诉导航网格代理移动到目标位置

只需将 [NavMeshAgent.destination](../ScriptReference/AI.NavMeshAgent-destination.html) 属性设置为您希望代理移动到的点，即可告诉代理开始计算路径。计算完成后，代理将自动沿路径移动，直至到达目标位置。下面的代码实现了一个简单的类，该类使用一个游戏对象来标记在 _Start_ 函数中分配给 _destination_ 属性的目标点。请注意，该脚本假定您已从 Editor 中添加并配置了导航网格代理 (NavMeshAgent) 组件。


````
	// MoveDestination.cs
		using UnityEngine;
	
		public class MoveDestination : MonoBehaviour {
	   
		   public Transform goal;
	   
		   void Start () {
			  NavMeshAgent agent = GetComponent<NavMeshAgent>();
			  agent.destination = goal.position; 
		   }
		} 
````

````
	// MoveDestination.js
		var goal: Transform;

		function Start() {
		  var agent: NavMeshAgent = GetComponent.<NavMeshAgent>();
		  agent.destination = goal.position; 
		}
````
