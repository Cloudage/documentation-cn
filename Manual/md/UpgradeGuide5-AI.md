##Unity 5.0 中的 AI

如果项目使用了 AI/导航网格 (NavMesh) 功能，在将项目从 Unity 4 升级到 Unity 5 时需要注意以下注意事项。

- 由于更改了分区，导航网格轮廓可能看起来不同（在狭窄的走廊/门道或类似情况下），这可能导致连接差异。通过调整导航网格构建的体素大小可解决此问题。
     
- 在调用“Stop”后设置导航网格代理 (NavMeshAgent) 的目标不会恢复代理 - 应显式调用“Resume”来恢复代理。
 
- NavMeshAgent.updatePosition：当 updatePosition 为 false 并移动代理变换组件时，代理位置不会更改。以前，代理位置将重置为变换位置 - 约束到附近的导航网格。

- NavMeshObstacle 组件：新创建的 NavMeshObstacle 组件的默认形状是一个盒体。选择的形状（盒体或胶囊体）现在适用于雕刻和避障。

- 不支持使用早期版本的 Unity 构建的导航网格。必须使用 Unity 5 重新构建。可使用以下脚本作为示例，了解如何为所有场景重新构建导航网格数据。

###重新烘焙脚本示例

````
# if UNITY_EDITOR
using System.Collections.Generic;
using System.Collections;
using System.IO;
using UnityEditor;
using UnityEngine;
using UnityEngine.AI;
public class RebakeAllScenesEditorScript
{
	[MenuItem ("Upgrade helper/Bake All Scenes")]
	public static void Bake()
	{
		List<string> sceneNames = SearchFiles (Application.dataPath, "*.unity");
		foreach (string f in sceneNames)
		{
			EditorApplication.OpenScene(f);
 
			// 重新烘焙导航网格数据
			NavMeshBuilder.BuildNavMesh ();
 
			EditorApplication.SaveScene ();
		}
	}
	static List<string> SearchFiles(string dir, string pattern)
	{
		List <string> sceneNames = new List <string>();
		foreach (string f in Directory.GetFiles(dir, pattern, SearchOption.AllDirectories))
		{
			sceneNames.Add (f);
		}
		return sceneNames;
	}
}
# endif
````
