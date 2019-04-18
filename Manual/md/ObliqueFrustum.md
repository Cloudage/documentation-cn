使用斜视锥体
========================


默认情况下，视锥体围绕摄像机的中心线对称安放，但这并不是必须的。视锥体可设置为“倾斜的”，即一侧与中心线的角度小于对侧与中心线的角度。效果就像拍摄打印的照片并切掉一条边。这种做法使得图像一侧的透视看起来更加紧凑，给人的印象是观察者非常靠近在该边缘处可见的对象。此功能的一个用法示例是赛车游戏，这种情况下的视锥体可能在其底边处变得扁平。因此，观察者看起来会更接近路面，突出了速度感。


![](../uploads/Main/ObliqueFrustum.png) 

虽然 Camera 类没有设置视锥体倾斜度的函数，但通过改变投影矩阵可以很容易实现这一点：
 
````
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	void SetObliqueness(float horizObl, float vertObl) {
		Matrix4x4 mat  = Camera.main.projectionMatrix;
		mat[0, 2] = horizObl;
		mat[1, 2] = vertObl;
		Camera.main.projectionMatrix = mat;
	}
} 
````
_C# 脚本示例_

````
function SetObliqueness(horizObl: float, vertObl: float) {
	var mat: Matrix4x4 = camera.projectionMatrix;
	mat[0, 2] = horizObl;
	mat[1, 2] = vertObl;
	camera.projectionMatrix = mat;
}

````
_JS 脚本示例_

幸运的是，没有必要了解投影矩阵如何使用它。horizObl 和 vertObl 值分别设置水平和垂直倾斜量。值为零表示无倾斜。正值使视锥体向右或向上移动，从而使左边或底边变平。负值使视锥体向左或向下移动，从而使视锥体的右边或顶边变平。如果将此脚本添加到摄像机并在游戏运行时将游戏切换到 Scene 视图，则可以直接看到效果；在检视面板中改变 horizObl 和 vertObl 的值时，摄像机视锥体的线框会发生变化。任一变量中的值为 1 或 -1 表示视锥体的一侧与中心线完全齐平。此范围之外的值也是允许使用的，但通常没有必要。
