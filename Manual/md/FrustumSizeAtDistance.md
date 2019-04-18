距摄像机一定距离的视锥体的大小
===========================================================


距摄像机一定距离的视锥体的横截面将在世界空间中定义一个构成可见区域的矩形。此形状有时可用于计算此矩形在给定距离处的大小，或者查找矩形为给定大小时所处的距离。例如，如果移动的摄像机需要始终将对象（例如玩家）完全保持在镜头中，则不得过于接近摄像机以免该对象的一部分被截断。

可使用以下公式来计算视锥体在给定距离处的高度（均以世界单位表示）：


	 var frustumHeight = 2.0f * distance * Mathf.Tan(camera.fieldOfView * 0.5f * Mathf.Deg2Rad);

...还可反转该过程以计算获得指定视锥体高度所需的距离：


	 var distance = frustumHeight * 0.5f / Mathf.Tan(camera.fieldOfView * 0.5f * Mathf.Deg2Rad);

当高度和距离已知时，也可以计算 FOV（视野）角度：
	

	 var camera.fieldOfView = 2.0f * Mathf.Atan(frustumHeight * 0.5f / distance) * Mathf.Rad2Deg;

这些计算都涉及到视锥体的高度，但这可以很容易从宽度（反之亦然）获得：



````
var frustumWidth = frustumHeight * camera.aspect;
var frustumHeight = frustumWidth / camera.aspect;
````
