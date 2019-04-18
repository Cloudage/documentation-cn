推拉变焦（也称为“伸缩变焦”效果）
======================================


推拉变焦 (Dolly Zoom) 是众所周知的摄像机同时向目标对象移动并将其缩小的视觉效果。结果是对象看起来大小大致相同，但场景中的其他所有对象都改变了视角。巧妙进行推拉变焦可实现突出目标对象的效果，因为它是场景中不会在图像中移位的唯一对象。或者，可特意快速执行变焦以产生迷失方向的效果。

刚好垂直装入视锥体内的对象将占据屏幕上看到的整个视图的高度。无论对象与摄像机的距离以及视野如何，都是如此。例如，您可以将摄像机移近对象，但随后加宽视野，使对象仍然刚好装在视锥体的高度内。该特定对象在屏幕上将显示相同的大小，但随着距离和 FOV（视野）的变化，其他所有对象都将改变大小。这就是推拉变焦效果的本质。


![](../uploads/Main/EqualFrustumHeight.png) 

通过代码创建该效果实际上就是在变焦开始时将视锥体高度保存在对象的位置。然后，摄像机移动时，找到它的新距离并调整 FOV 以使其在对象位置保持相同高度。通过以下代码可实现这一点：

````
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	public Transform target;
	public Camera camera;
	
	private float initHeightAtDist;
	private bool dzEnabled;

	// 计算距摄像机一定距离的视锥体高度。
	void FrustumHeightAtDistance(float distance) {
		return 2.0f * distance * Mathf.Tan(camera.fieldOfView * 0.5f * Mathf.Deg2Rad);
	}

	// 计算在给定距离处获得给定视锥体高度所需的 FOV。
	void FOVForHeightAndDistance(float height, float distance) {
		return 2.0f * Mathf.Atan(height * 0.5f / distance) * Mathf.Rad2Deg;
	}

	//启动推拉变焦效果。
	void StartDZ() {
		var distance = Vector3.Distance(transform.position, target.position);
		initHeightAtDist = FrustumHeightAtDistance(distance);
		dzEnabled = true;
	}
	
	// 关闭推拉变焦。
	void StopDZ() {
		dzEnabled = false;
	}
	
	void Start() {
		StartDZ();
	}

	void Update () {
		if (dzEnabled) {
			//测量新距离并相应重新调整 FOV。
			var currDistance = Vector3.Distance(transform.position, target.position);
			camera.fieldOfView = FOVForHeightAndDistance(initHeightAtDist, currDistance);
		}
		
		//采用简单控制方式，允许使用向上/向下箭头来移入和移出摄像机。
		transform.Translate(Input.GetAxis("Vertical") * Vector3.forward * Time.deltaTime * 5f);
	}
}
````
_C# 脚本示例_

````
var target: Transform;

private var initHeightAtDist: float;
private var dzEnabled: boolean;


// 计算距摄像机给定距离的视锥体高度。
function FrustumHeightAtDistance(distance: float) {
	return 2.0 * distance * Mathf.Tan(camera.fieldOfView * 0.5 * Mathf.Deg2Rad);
}


//计算在给定距离处获得给定视锥体高度所需的 FOV。
function FOVForHeightAndDistance(height: float, distance: float) {
	return 2 * Mathf.Atan(height * 0.5 / distance) * Mathf.Rad2Deg;
}


//启动推拉变焦效果。
function StartDZ() {
	var distance = Vector3.Distance(transform.position, target.position);
	initHeightAtDist = FrustumHeightAtDistance(distance);
	dzEnabled = true;
}


// 关闭推拉变焦。
function StopDZ() {
	dzEnabled = false;
}


function Start() {
	StartDZ();
}


function Update () {
	if (dzEnabled) {
		//测量新距离并相应重新调整 FOV。
		var currDistance = Vector3.Distance(transform.position, target.position);
		camera.fieldOfView = FOVForHeightAndDistance(initHeightAtDist, currDistance);
	}
	
	//采用简单控制方式，允许使用向上/向下箭头来移入和移出摄像机。
	transform.Translate(Input.GetAxis("Vertical") * Vector3.forward * Time.deltaTime * 5);
}

````
_JS 脚本示例_
