#将代理移动到鼠标单击的位置

此脚本允许通过在对象表面上单击鼠标来选择导航网格上的目标点。单击位置由_射线投射_确定，而非像将激光束指向对象来查看其所在位置（有关此技术的完整描述，请参阅[摄像机射线](CameraRays.html)页面）。由于 [GetComponent](../ScriptReference/GameObject.GetComponent.html) 函数的执行速度相当慢，因此该脚本在 _Start_ 函数期间将其结果存储在变量中，而不是在 _Update_ 中重复调用它。

````
	// MoveToClickPoint.cs
		using UnityEngine;
		using UnityEngine.AI;
	
		public class MoveToClickPoint : MonoBehaviour {
			NavMeshAgent agent;
		
			void Start() {
				agent = GetComponent<NavMeshAgent>();
			}
		
			void Update() {
				if (Input.GetMouseButtonDown(0)) {
					RaycastHit hit;
				
					if (Physics.Raycast(Camera.main.ScreenPointToRay(Input.mousePosition), out hit, 100)) {
						agent.destination = hit.point;
					}
				}
			}
		} 
````
		
````
	//MoveToClickPoint.js
		var agent: NavMeshAgent;
	
		function Start() {
			agent = GetComponent.<NavMeshAgent>();
		}

		function Update() {
			if (Input.GetMouseButtonDown(0)) {
				var hit: RaycastHit;
		
				if (Physics.Raycast(Camera.main.ScreenPointToRay(Input.mousePosition), hit, 100)) {
					agent.destination = hit.point;
				}
			}
		}
````
