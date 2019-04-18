摄像机射线
====================


[了解视锥体](UnderstandingFrustum.html)部分说明了摄像机视图中的任何一点都对应于世界空间中的一条线。有时使用这条线的数学表示形式是有用的，Unity 能够以 [Ray](../ScriptReference/Ray.html) 对象的形式提供该表示形式。Ray 始终对应于视图中的一个点，因此 Camera 类提供 [ScreenPointToRay](../ScriptReference/Camera.ScreenPointToRay.html) 和 [ViewportPointToRay](../ScriptReference/Camera.ViewportPointToRay.html) 函数。两者之间的区别在于 ScreenPointToRay 期望以像素坐标的形式提供该点，而 ViewportPointToRay 则接受 0..1 范围内的标准化坐标（其中 0 表示视图的左下角，1 表示右上角）。这些函数中的每一个函数都返回由一个原点和一个矢量（该矢量显示从该原点出发的线条方向）组成的 Ray。射线 (Ray) 源自近裁剪面而不是摄像机 (Camera) 的 transform.position 点。

射线投射
----------


来自摄像机的射线最常见的用途是将[射线投射 (raycast)](../ScriptReference/Physics.Raycast.html) 到场景中。射线投射从原点沿着射线方向发送假想的“激光束”，直至命中场景中的碰撞体。随后会返回有关该对象和 [RaycastHit](../ScriptReference/RaycastHit.html) 对象内的投射命中点的信息。这是一种基于对象在屏幕上的图像来定位对象的非常有用的方法。例如，可使用以下代码确定鼠标位置处的对象：

_C# 脚本示例：_

````
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	public Camera camera;

	void Start(){
		RaycastHit hit;
		Ray ray = camera.ScreenPointToRay(Input.mousePosition);
		
		if (Physics.Raycast(ray, out hit)) {
			Transform objectHit = hit.transform;
			
			// 对射线投射命中的对象执行一些操作。
		}
	}
}
````

_JS 脚本示例：_

````
var hit: RaycastHit;
var ray: Ray = camera.ScreenPointToRay(Input.mousePosition);

if (Physics.Raycast(ray, hit)) {
	var objectHit: Transform = hit.transform;
	
	// 对射线投射命中的对象执行一些操作。
}

````

沿着射线移动摄像机
-----------------------------


获取对应于屏幕位置的射线再沿着该射线移动摄像机有时很有用。例如，需要允许用户使用鼠标选择对象，然后放大该对象，同时将其“固定”到鼠标下的相同屏幕位置（这种操作可能很有用，例如，当摄像机正在查看战术地图时）。执行此操作的代码非常简单：

_C# 脚本示例：_

````
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	public bool zooming;
	public float zoomSpeed;
	public Camera camera;

	void Update() {
		if (zooming) {
			Ray ray = camera.ScreenPointToRay(Input.mousePosition);
			float zoomDistance = zoomSpeed * Input.GetAxis("Vertical") * Time.deltaTime;
			camera.transform.Translate(ray.direction * zoomDistance, Space.World);
		}
	}
}
````


_JS 脚本示例：_

````
var zooming: boolean;
var zoomSpeed: float;

if (zooming) {
	var ray: Ray = camera.ScreenPointToRay(Input.mousePosition);
	zoomDistance = zoomSpeed * Input.GetAxis("Vertical") * Time.deltaTime;
	camera.transform.Translate(ray.direction * zoomDistance, Space.World);
}

````
