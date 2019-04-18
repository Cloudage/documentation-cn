#使用多个摄像机

当一个 Unity 场景刚创建完毕时，它只包含一个摄像机，这在大多数情况下就已经足够了。但是，您也可以在场景中添加任意数量的摄像机，还能以不同的方式组合它们的视图，如下所述。


##切换摄像机

默认情况下，摄像机会用它所渲染的画面覆盖整个屏幕，因此同一时刻只能看到一个摄像机的画面（即 _depth_ 属性具有最高值的摄像机）。通过在脚本中禁用一个摄像机并启用另一个，即可从一个摄像机的镜头“切”到另一个，从而得到场景的多个镜头画面。在某些情况下，例如要在俯瞰地图视角和第一人称视角之间切换时，就可以采用这种方法。

````
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	public Camera firstPersonCamera;
	public Camera overheadCamera;

	public void ShowOverheadView() {
		firstPersonCamera.enabled = false;
		overheadCamera.enabled = true;
	}
	
	public void ShowFirstPersonView() {
		firstPersonCamera.enabled = true;
		overheadCamera.enabled = false;
	}
} 
````
_C# 脚本示例_

````
var firstPersonCamera: Camera;
var overheadCamera: Camera;

function ShowOverheadView() {
	firstPersonCamera.enabled = false;
	overheadCamera.enabled = true;
}

function ShowFirstPersonView() {
	firstPersonCamera.enabled = true;
	overheadCamera.enabled = false;
}
````
_JS 脚本示例_


##渲染画中画

通常情况下，您希望至少有一个摄像机视图覆盖整个屏幕（默认设置），但在屏幕的一个小区域内显示另一个视图往往也很有用。例如，您可能会在驾驶游戏中显示一个后视镜，或者在第一人视角的屏幕角落里显示俯瞰小地图。您可以使用摄像机的 _Viewport Rect_ 属性设置摄像机在屏幕上的矩形的大小。

视口矩形的坐标相对于屏幕经过“标准化”。底边和左边是 0.0 坐标，而顶边和右边是 1.0。坐标值为 0.5 表示位于中间位置。除了视口大小之外，还应将具有较小视图的摄像机的 _depth_ 属性设置为一个高于背景摄像机的值。确切的值无关紧要，总之规则是具有较高 depth 值的摄像机的渲染画面会覆盖在较低值摄像机的渲染画面之上。


