#协程

调用函数时，函数将运行到完成状态，然后返回。这实际上意味着在函数中发生的任何动作都必须在单帧更新内发生；函数调用不能用于包含程序性动画或随时间推移的一系列事件。例如，假设需要逐渐减少对象的 Alpha（不透明度）值，直至对象变得完全不可见。

````
void Fade() {
	for (float f = 1f; f >= 0; f -= 0.1f) {
		Color c = renderer.material.color;
		c.a = f;
		renderer.material.color = c;
	}
}

````

就目前而言，Fade 函数不会产生期望的效果。为了使淡入淡出过程可见，必须通过一系列帧降低 Alpha 以显示正在渲染的中间值。但是，该函数将完全在单个帧更新中执行。这种情况下，永远不会看到中间值，对象会立即消失。

可以通过向 Update 函数添加代码（此代码逐帧执行淡入淡出）来处理此类情况。但是，使用协程来执行此类任务通常会更方便。

协程就像一个函数，能够暂停执行并将控制权返还给 Unity，然后在下一帧继续执行。在 C# 中，声明协程的方式如下：



````
IEnumerator Fade() {
	for (float f = 1f; f >= 0; f -= 0.1f) {
		Color c = renderer.material.color;
		c.a = f;
		renderer.material.color = c;
		yield return null;
	}
}

````

此协程本质上是一个用返回类型 IEnumerator 声明的函数，并在主体中的某个位置包含 yield return 语句。yield return 行是暂停执行并随后在下一帧恢复的点。要将协程设置为运行状态，必须使用 [StartCoroutine](../ScriptReference/MonoBehaviour.StartCoroutine.html) 函数：



````
void Update() {
	if (Input.GetKeyDown("f")) {
		StartCoroutine("Fade");
	}
}

````

在 UnityScript 中，情况稍微简单一些。包含 yield 语句的任何函数都视为协程，并且无需显式声明 IEnumerator 返回类型：



````
function Fade() {
	for (var f = 1.0; f >= 0; f -= 0.1) {
		var c = renderer.material.color;
		c.a = f;
		renderer.material.color = c;
		yield;
	}
}

````

此外，为了在 UnityScript 中启动协程，可直接调用协程，就像是普通函数一样：



````
function Update() {
	if (Input.GetKeyDown("f")) {
		Fade();
	}
}

````

您会注意到 Fade 函数中的循环计数器能够在协程的生命周期内保持正确值。实际上，在 yield 语句之间可以正确保留任何变量或参数。

默认情况下，协程将在执行 yield 后的帧上恢复，但也可以使用 [WaitForSeconds](../ScriptReference/WaitForSeconds.html) 来引入时间延迟：



````
IEnumerator Fade() {
	for (float f = 1f; f >= 0; f -= 0.1f) {
		Color c = renderer.material.color;
		c.a = f;
		renderer.material.color = c;
		yield return new WaitForSeconds(.1f);
	}
}

````

在 UnityScript 中：



````
function Fade() {
	for (var f = 1.0; f >= 0; f -= 0.1) {
		var c = renderer.material.color;
		c.a = f;
		renderer.material.color = c;
		yield WaitForSeconds(0.1);
	}
}

````

可通过这样的协程在一段时间内传播效果，但也可将其作为一种有用的优化。游戏中的许多任务需要定期执行，最容易想到的方法是将任务包含在 Update 函数中。但是，通常情况下，每秒将多次调用该函数。不需要以这样的频繁程度重复任务时，可以将其放在协程中来进行定期更新，而不是每一帧都更新。这方面的一个示例可能是在附近有敌人时向玩家发出的警报。此代码可能如下所示：



````
function ProximityCheck() {
	for (int i = 0; i < enemies.Length; i++) {
		if (Vector3.Distance(transform.position, enemies[i].transform.position) < dangerDistance) {
				return true;
		}
	}
	
	return false;
}

````

如果有很多敌人，那么每帧都调用此函数可能会带来很大开销。但是，可以使用协程，每十分之一秒调用一次：



````
IEnumerator DoCheck() {
	for(;;) {
		ProximityCheck;
		yield return new WaitForSeconds(.1f);
	}
}

````

这将大大减少所进行的检查次数，而不会对游戏运行过程产生任何明显影响。

注意：禁用 MonoBehaviour 时，不会停止协程，仅在明确销毁 MonoBehaviour 时才会停止协程。可以使用 MonoBehaviour.StopCoroutine 和 MonoBehaviour.StopAllCoroutines 来停止协程。销毁 MonoBehaviour 时，也会停止协程。
