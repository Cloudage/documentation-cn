#车轮碰撞体教程
 
车轮碰撞体 (Wheel Collider) 组件由 PhysX 3 车辆 SDK 提供支持。

本教程将介绍如何创建一辆具备基本功能的汽车。

首先选择 __GameObject__ &gt; __3D Object__ &gt; __Plane__。这是将汽车行驶的地面。为简单起见，请确保地面的变换为 __0__（在 Inspector 窗口的 Transform 组件上，单击 Settings 齿轮，然后单击 __Reset__）。将 Transform 组件的 Scale 字段增大到 __100__ 以放大平面。

## 创建一个基本的汽车骨架

1.首先，添加一个游戏对象作为汽车的根游戏对象。为此，请选择 __GameObject__ > __Create Empty__。将游戏对象的名称更改为 `car_root`。
1.向 `car_root` 中添加一个 3D 物理刚体组件。对于默认悬架设置而言，默认质量 1kg 太轻；请将其更改为 1500kg 以大幅增加其质量。
1.然后，创建汽车车身碰撞体。选择 __GameObject__ &gt; __3D Object__ &gt; __Cube__。将此立方体设为 `car_root` 下面的一个子游戏对象。将 Transform 重置为 __0__ 以使其在局部空间中完美对齐。汽车沿 Z 轴定向，因此请将 Transform 中的 __Z__ __Scale__ 设置为 __3__。
1.添加车轮根节点。依次选择 `car_root` 和 __GameObject__ &gt; __Create Empty Child__。将名称更改为 `wheels`。重置其中的 Transform 组件。此游戏对象不是必需的，但对于后面的调整和调试很有用。
1.要创建第一个车轮，请选择 `wheels` 游戏对象，然后选择 __GameObject__ &gt; __Create Empty Child__，并将其命名为 `frontLeft`。重置 Transform 组件，然后将 Transform __Position__ __X__ 设置为 -1，将 __Y__ 设置为 0，并将 __Z__ 设置为 1。要向车轮添加碰撞体，请选择 __Add component__ &gt; __Physics__ &gt; __Wheel Collider__。
1.复制 `frontLeft` 游戏对象。将 __Transform__ 的 __X__ 位置更改为 1。将名称更改为 `frontRight`。
1.同时选择 `frontLeft` 和 `frontRight` 游戏对象。复制这两个游戏对象。将这两个游戏对象的 __Transform__ __Z__ 位置更改为 -1。将名称分别更改为 `rearLeft` 和 `rearRight`。
1.最后，选择 `car_root` 游戏对象，并使用移动工具将其抬高到略高于地面的位置。

现在应该能看到如下所示的结果：

![](../uploads/Main/WheelColliderTutorial.png) 
     
为了使这辆车真正可驾驶，需要为其编写一个控制器。以下代码示例将用作控制器：

```
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
	
public class SimpleCarController : MonoBehaviour {
	public List<AxleInfo> axleInfos; // 关于每个轴的信息
	public float maxMotorTorque; // 电机可对车轮施加的最大扭矩
	public float maxSteeringAngle; // 车轮的最大转向角
		
	public void FixedUpdate()
	{
		float motor = maxMotorTorque * Input.GetAxis("Vertical");
		float steering = maxSteeringAngle * Input.GetAxis("Horizontal");
			
		foreach (AxleInfo axleInfo in axleInfos) {
			if (axleInfo.steering) {
				axleInfo.leftWheel.steerAngle = steering;
				axleInfo.rightWheel.steerAngle = steering;
			}
			if (axleInfo.motor) {
				axleInfo.leftWheel.motorTorque = motor;
				axleInfo.rightWheel.motorTorque = motor;
			}
		}
	}
}
	
[System.Serializable]
public class AxleInfo {
	public WheelCollider leftWheel;
	public WheelCollider rightWheel;
	public bool motor; // 此车轮是否连接到电机？
	public bool steering; // 此车轮是否施加转向角？
}
```

在 `car_root` 游戏对象上创建新的 C# 脚本 (__Add Component__ &gt; __New Script__)，将该示例代码复制到脚本文件中并保存。可以按如下所示调整脚本参数；尝试使用不同设置并进入播放模式以测试结果。

以下设置作为汽车控制器非常有效：

![](../uploads/Main/WheelColliderSettings.png) 

单个车辆实例上最多可以有 20 个车轮，每个车轮都施加转向、电机或制动扭矩。

接下来看看可视车轮。如您所见，车轮碰撞体不会将模拟的车轮位置和旋转反向应用于车轮碰撞体的变换，因此添加可视车轮需要一些技巧。

这里需要一些车轮几何体。可以用圆柱体制作简单的车轮形状。可通过多种方法来添加可视车轮：制作车轮后必须在脚本属性中手动分配可视车轮，或者编写一些逻辑来自动查找相应的可视车轮。本教程遵循第二种方法。将可视车轮附加到车轮碰撞体游戏对象。

接下来，更改控制器脚本：

```
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

[System.Serializable]
public class AxleInfo {
	public WheelCollider leftWheel;
	public WheelCollider rightWheel;
	public bool motor;
	public bool steering;
}
	 
public class SimpleCarController : MonoBehaviour {
	public List<AxleInfo> axleInfos; 
	public float maxMotorTorque;
	public float maxSteeringAngle;
	 
	// 查找相应的可视车轮
	// 正确应用变换
	public void ApplyLocalPositionToVisuals(WheelCollider collider)
	{
		if (collider.transform.childCount == 0) {
			return;
		}
	 
		Transform visualWheel = collider.transform.GetChild(0);
	 
		Vector3 position;
		Quaternion rotation;
		collider.GetWorldPose(out position, out rotation);
	 
		visualWheel.transform.position = position;
		visualWheel.transform.rotation = rotation;
	}
	 
	public void FixedUpdate()
	{
		float motor = maxMotorTorque * Input.GetAxis("Vertical");
		float steering = maxSteeringAngle * Input.GetAxis("Horizontal");
	 
		foreach (AxleInfo axleInfo in axleInfos) {
			if (axleInfo.steering) {
				axleInfo.leftWheel.steerAngle = steering;
				axleInfo.rightWheel.steerAngle = steering;
			}
			if (axleInfo.motor) {
				axleInfo.leftWheel.motorTorque = motor;
				axleInfo.rightWheel.motorTorque = motor;
			}
			ApplyLocalPositionToVisuals(axleInfo.leftWheel);
			ApplyLocalPositionToVisuals(axleInfo.rightWheel);
		}
	}
}
```

车轮碰撞体组件的一个重要参数是 __Force App Point Distance__。此参数是从静止车轮的底部到车轮受力点的距离。默认值为 0，表示在静止车轮的底部施加力，但明智的做法是将此点定位在略低于汽车质心的位置。

**注意**：要查看车轮碰撞体的实际应用，请下载 [Vehicle Tools](https://www.assetstore.unity3d.com/en/#!/content/83660) 资源包，其中包含了用于装配轮式车辆以及为车轮碰撞体创建悬架的工具。

---

* <span class="page-edit">2017-11-24  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
