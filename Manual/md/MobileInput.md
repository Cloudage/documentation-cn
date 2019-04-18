#移动设备输入


在移动设备上，[Input](../ScriptReference/Input.html) 类提供对触摸屏、加速度计和地理/位置输入的访问。

通过 [iOS 键盘](MobileKeyboard.html)可以访问移动设备上的键盘。

###多点触控屏幕


iPhone 和 iPod Touch 设备最多可跟踪五根手指同时触摸屏幕。可通过访问 [Input.touches](../ScriptReference/Input-touches.html) 属性数组来检索在最后一帧期间触摸屏幕的每根手指的状态。

Android 设备对其跟踪的手指数量没有统一限制。相反，此限制因设备而异，可能是旧设备上的双手指触摸到某些新设备上的五指触摸。

每根手指的触摸由 [Input.Touch](../ScriptReference/Touch.html) 数据结构表示：

|**_属性：_** |**_功能：_** |
|:---|:---|
|__fingerId__ |触摸的唯一索引。|
|__position__ |触摸的屏幕位置。|
|__deltaPosition__ |自上一帧以来的屏幕位置变化。|
|__deltaTime__ |自上次状态变化以来经过的时间。|
|__tapCount__ |iPhone/iPad 屏幕能够区分用户的快速手指点击。此计数器将记录用户在不将手指移到侧面的情况下点击屏幕的次数。Android 设备不计算点击次数，此字段始终为 1。|
|__phase__ |描述所谓的“阶段”或触摸状态。有助于确定触摸刚开始、用户移动了手指还是刚抬起手指。|

允许的阶段如下：

| | |
|:---|:---|
|__Began__ |手指刚触摸了屏幕。|
|__Moved__ |手指在屏幕上进行了移动。|
|__Stationary__ |手指正在触摸屏幕但自上一帧以来尚未移动。|
|__Ended__ |从屏幕上抬起了手指。这是触摸的最后一个阶段。|
|__Canceled__ |由于某种情况，例如用户将设备放在他们的脸部或同时执行超过五次触摸，系统取消了对触摸的跟踪。这是触摸的最后一个阶段。|

以下是一个示例脚本，只要用户点击屏幕，就会发出一条射线：


````
var particle : GameObject;
function Update () {
	for (var touch : Touch in Input.touches) {
		if (touch.phase == TouchPhase.Began) {
			// 从当前触摸坐标构造一条射线
			var ray = Camera.main.ScreenPointToRay (touch.position);
			if (Physics.Raycast (ray)) {
				// 如果命中，则创建一个粒子
				Instantiate (particle, transform.position, transform.rotation);
			}
		}
	}
}


````

###鼠标模拟
除了原生触摸支持，Unity iOS/Android 还提供鼠标模拟功能。可使用标准 [Input](../ScriptReference/Input.html) 类中的鼠标功能。请注意，根据设计，iOS/Android 设备支持多指触摸。使用鼠标功能时仅支持单指触摸。此外，移动设备上的手指触摸可从一个区域移动到另一个区域，而它们之间没有移动。移动设备上的鼠标模拟将提供移动，因此与触摸输入相比非常不同。建议在早期开发期间使用鼠标模拟，但要尽快使用触摸输入。

###加速度计


随着移动设备的移动，内置的加速度计会报告沿三维空间中的
三个主轴的线性加速度变化。硬件
将沿每个轴的加速度直接报告为重力值。值 1.0
表示沿给定轴约 +1g 的荷载，而值 -1.0
表示 -1g。如果将设备竖直握住（主屏幕按钮位于
底部）举到您正前方，则右侧为正 X 轴，上方为正 Y 轴，
指向您的方向为正 Z 轴。

可通过访问 [Input.acceleration](../ScriptReference/Input-acceleration.html) 属性来检索加速度计值。

以下是使用加速度计移动对象的示例脚本：


````
var speed = 10.0;
function Update () {
	var dir : Vector3 = Vector3.zero;

	// 我们假设设备与地面平行，
	// 主屏幕按钮位于右侧

	// 将设备的加速度轴重新映射到游戏坐标：
	// 1) 设备的 XY 平面映射到 XZ 平面
	// 2) 绕 Y 轴旋转 90 度
	dir.x = -Input.acceleration.y;
	dir.z = Input.acceleration.x;

	// 将加速度矢量限制为单位球体
	if (dir.sqrMagnitude > 1)
		dir.Normalize();

	// 使其每秒移动 10 米而不是每帧 10 米...
	dir *= Time.deltaTime;

	//移动对象
	transform.Translate (dir * speed);
}


````

###低通滤波器
加速度计读数可能不稳定且噪声很大。对信号应用低通滤波可以使其平滑并消除高频噪声。

以下脚本显示了如何将低通滤波应用于加速度计读数：


````
var AccelerometerUpdateInterval : float = 1.0 / 60.0;
var LowPassKernelWidthInSeconds : float = 1.0;

private var LowPassFilterFactor : float = AccelerometerUpdateInterval / LowPassKernelWidthInSeconds; // 可调整
private var lowPassValue : Vector3 = Vector3.zero;
function Start () {
	lowPassValue = Input.acceleration;
}

function LowPassFilterAccelerometer() : Vector3 {
	lowPassValue = Vector3.Lerp(lowPassValue, Input.acceleration, LowPassFilterFactor);
	return lowPassValue;
}


````

`LowPassKernelWidthInSeconds` 的值越大，滤波值向当前输入样本收敛的速度越慢（反之亦然）。

###在读取加速度计的读数时，我希望尽可能精确。该怎么办？
读取 [Input.acceleration](../ScriptReference/Input-acceleration.html) 变量不等于对硬件进行采样。简而言之，Unity 以 60Hz 的频率对硬件进行采样并将结果存储到变量中。实际情况有点复杂：如果 CPU 负载很大，加速度计采样不会以一致的时间间隔发生。结果，系统可能在一帧期间报告 2 个样本，然后在下一帧期间报告 1 个样本。

您可以访问加速度计在帧内执行的所有测量。以下代码将展示在最后一帧内收集的所有加速度计事件的简单平均值：



````
var period : float = 0.0;
var acc : Vector3 = Vector3.zero;
for (var evnt : iPhoneAccelerationEvent in iPhoneInput.accelerationEvents) {
	acc += evnt.acceleration * evnt.deltaTime;
	period += evnt.deltaTime;
}
if (period > 0)
	acc *= 1.0/period;
return acc;


````

