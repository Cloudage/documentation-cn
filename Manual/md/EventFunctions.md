事件函数
===============


Unity 中的脚本与传统的程序概念不同。在传统程序中，代码在循环中连续运行，直到完成任务。相反，Unity 通过调用在脚本中声明的某些函数来间歇地将控制权交给脚本。函数执行完毕后，控制权将交回 Unity。这些函数由 Unity 激活以响应游戏中发生的事件，因此称为事件函数。Unity 使用一种命名方案来标识要对特定事件调用的函数。例如，先前介绍过的 Update 函数（在帧更新发生之前调用）和 Start 函数（在对象的第一次帧更新之前立即调用）。Unity 中提供了大量其他事件函数；可在 [MonoBehaviour](../ScriptReference/MonoBehaviour.html) 类的脚本参考页面中找到事件函数的完整列表以及详细的事件函数用法说明。以下是一些最常见和最重要的事件。


常规更新事件
---------------------


游戏很像动画，其中的动画帧是动态生成的。游戏编程中的一个关键概念是在渲染每帧之前改变游戏对象的位置、状态和行为。[Update](../ScriptReference/MonoBehaviour.Update.html) 函数是 Unity 中包含这种代码的主要位置。在渲染帧之前以及计算动画之前都会调用 Update 函数。



````
void Update() {
	float distance = speed * Time.deltaTime * Input.GetAxis("Horizontal");
	transform.Translate(Vector3.right * distance);
}

````

物理引擎也采用与帧渲染类似的方式以离散时间步骤进行更新。在每次物理更新之前都会调用一个称为 [FixedUpdate](../ScriptReference/MonoBehaviour.FixedUpdate.html) 的单独事件函数。由于物理更新和帧更新不会以相同频率进行，所以如果将物理代码放在 FixedUpdate 函数而不是 Update 中，此代码将产生更准确的结果。
	


````
void FixedUpdate() {
	Vector3 force = transform.forward * driveForce * Input.GetAxis("Vertical");
	rigidbody.AddForce(force);
}

````

为场景中的所有对象调用 Update 和 FixedUpdate 函数之后以及计算所有动画之后，如果能进行其他更改，也会很有用。一个例子是摄像机应该聚焦于目标对象；必须在目标对象移动后才能调整摄像机的方向。另一个例子是脚本代码应该覆盖动画的效果（例如，让角色的头部看向场景中的目标对象）。可使用 [LateUpdate](../ScriptReference/MonoBehaviour.LateUpdate.html) 函数来处理这些类型的情况。



````
void LateUpdate() {
	Camera.main.transform.LookAt(target.transform);
}

````


初始化事件
---------------------


如果能在游戏运行过程中进行任何更新之前调用初始化代码，通常会很有用。在第一帧之前或开始对象的物理更新之前需要调用 [Start](../ScriptReference/MonoBehaviour.Start.html) 函数。场景加载时会为场景中的每个对象调用 [Awake](../ScriptReference/MonoBehaviour.Awake.html) 函数。请注意，虽然各种对象的 Start 和 Awake 函数的调用顺序是任意的，但在调用第一个 Start 之前，所有 Awake 都要完成。这意味着 Start 函数中的代码可以利用先前在 Awake 阶段执行的其他初始化。


GUI 事件
----------


Unity 有一个系统用于渲染场景中主要操作的 GUI 控件，并响应对这些控件的点击。此代码的处理方式与正常的帧更新有些不同，因此应将此代码置于定期调用的 [OnGUI](../ScriptReference/MonoBehaviour.OnGUI.html) 函数中。



````
void OnGUI() {
	GUI.Label(labelRect, "Game Over");
}

````

此外，还可以检测场景中出现的游戏对象上发生的鼠标事件。使用此功能可以定位武器或显示当前在鼠标指针下的角色的相关信息。借助一系列 OnMouseXXX 事件函数（例如 [OnMouseOver](../ScriptReference/MonoBehaviour.OnMouseOver.html)、[OnMouseDown](../ScriptReference/MonoBehaviour.OnMouseDown.html)）可以让脚本对用户的鼠标操作做出反应。例如，如果指针位于特定对象上时按下鼠标按钮，则会调用该对象的脚本（如果存在）中的 OnMouseDown 函数。


物理事件
--------------


物理引擎将通过调用该对象的脚本上的事件函数来报告对象的碰撞情况。在进行接触、保持接触和中断接触时，将调用 [OnCollisionEnter](../ScriptReference/MonoBehaviour.OnCollisionEnter.html)、[OnCollisionStay](../ScriptReference/MonoBehaviour.OnCollisionStay.html) 和 [OnCollisionExit](../ScriptReference/MonoBehaviour.OnCollisionExit.html) 函数。对象的碰撞体配置为触发器（即，碰撞体只检测某物何时进入而不进行物理反应）时，将调用相应的 [OnTriggerEnter](../ScriptReference/MonoBehaviour.OnTriggerEnter.html)、[OnTriggerStay](../ScriptReference/MonoBehaviour.OnTriggerStay.html) 和 [OnTriggerExit](../ScriptReference/MonoBehaviour.OnTriggerExit.html) 函数。如果在物理更新期间检测到多次接触，可能连续多次调用这些函数，因此会将一个参数传递给函数，从而提供碰撞的详细信息（位置、进入对象标识等）。



````
void OnCollisionEnter(otherObj: Collision) {
	if (otherObj.tag == "Arrow") {
		ApplyDamage(10);
	}
}

````
