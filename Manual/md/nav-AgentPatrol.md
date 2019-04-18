#在一组点之间进行代理巡逻

许多游戏都有 NPC 负责在游戏区域周围自动巡逻。使用导航系统可实现此行为，但它比标准寻路方法稍微复杂一些；标准寻路方法仅使用两点之间的最短路径就可以实现有限且可预测的巡逻路线。为获得更有说服力的巡逻模式，可保留一组“有用”的关键点让 NPC 以某种顺序通过并访问它们。例如，在迷宫中，可将关键巡逻点放置在交叉点和拐角处，从而确保代理检查每个走廊。对于办公楼，关键点可能是各个办公室和其他房间。

![标有关键巡逻点的迷宫](../uploads/Main/NavPatrolMaze.svg)

理想的巡逻点序列将取决于所需的 NPC 行为方式。例如，机器人可能只是按照有条不紊的顺序访问这些点，而人类守卫可能会尝试通过使用更随机的模式来发现玩家。可使用下面显示的代码实现机器人的简单行为。

应使用公共变换数组将巡逻点提供给脚本。可从检视面板分配此数组，并使用游戏对象来标记巡逻点的位置。_GotoNextPoint_ 函数可设置代理的目标点（也开始移动代理），然后选择将在下次调用时使用的新目标。就目前而言，该代码将按照巡逻点在数组中出现的顺序循环遍历巡逻点，但您可以轻松修改这种设置，例如使用 [Random.Range](../ScriptReference/Random.Range.html) 来随机选择数组索引。

在 _Update_ 函数中，该脚本使用 [remainingDistance](../ScriptReference/AI.NavMeshAgent-remainingDistance.html) 属性检查代理与目标的接近程度。当此距离非常小时，将调用 _GotoNextPoint_ 来启动巡逻的下一段。
 

````
	// Patrol.cs
		using UnityEngine;
		using UnityEngine.AI;
		using System.Collections;


		public class Patrol : MonoBehaviour {

			public Transform[] points;
			private int destPoint = 0;
			private NavMeshAgent agent;


			void Start () {
				agent = GetComponent<NavMeshAgent>();

				// 禁用自动制动将允许点之间的
				// 连续移动（即，代理在接近目标点时
				// 不会减速）。
				agent.autoBraking = false;

				GotoNextPoint();
			}


			void GotoNextPoint() {
				// 如果未设置任何点，则返回
				if (points.Length == 0)
					return;

				//将代理设置为前往当前选定的目标。
				agent.destination = points[destPoint].position;

				//选择数组中的下一个点作为目标，
				// 如有必要，循环到开始。
				destPoint = (destPoint + 1) % points.Length;
			}


			void Update () {
				//当代理接近当前目标点时，
				// 选择下一个目标点。
				if (!agent.pathPending && agent.remainingDistance < 0.5f)
					GotoNextPoint();
			}
		}
````
		
````
	// Patrol.js
		var points: Transform[];
		var destPoint: int = 0;
		var agent: NavMeshAgent;


		function Start() {
			agent = GetComponent.<NavMeshAgent>();

			// 禁用自动制动将允许点之间的
			// 连续移动（即，代理在接近目标点时
			// 不会减速）。
			agent.autoBraking = false;

			GotoNextPoint();
		}


		function GotoNextPoint() {
			// 如果未设置任何点，则返回
			if (points.Length == 0)
				return;
			
			//将代理设置为前往当前选定的目标。
			agent.destination = points[destPoint].position;

			//选择数组中的下一个点作为目标，
			// 如有必要，循环到开始。
			destPoint = (destPoint + 1) % points.Length;
		}


		function Update() {
			//当代理接近当前目标点时，
			// 选择下一个目标点。
			if (!agent.pathPending && agent.remainingDistance < 0.5f)
				GotoNextPoint();
		}
````
