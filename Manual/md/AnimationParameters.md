动画参数
====================


动画参数是在 Animator Controller 中定义的变量，可从脚本访问这些变量并向其赋值。这是脚本控制或影响状态机流程的方法。

例如，可通过[动画曲线](animeditor-AnimationCurves.html)更新参数的值，然后从脚本访问参数以便可改变音效的音高（就像它是一段动画一样）。同样，脚本可设置被 Mecanim 拾取的参数值。例如，脚本可设置参数来控制[混合树](class-BlendTree.html)。

可使用 Animator 窗口的 Parameters 部分来设置默认参数值（可在 Animator 窗口的右上角进行选择）。这些参数可分为四个基本类型：

![](../uploads/Main/AnimationEditorParametersSection.png) 

* _Int_ - 整数
* _Float_ - 带小数部分的数字
* _Bool_ - true 或 false 值（由复选框表示）
* _Trigger_ - 当被过渡使用时，由控制器重置的布尔值参数（以圆形按钮表示）


可使用以下 Animator 类中的函数，从脚本为参数赋值：[SetFloat](../ScriptReference/Animator.SetFloat.html)、[SetInt](../ScriptReference/Animator.SetInteger.html)、[SetBool](../ScriptReference/Animator.SetBool.html)、[SetTrigger](../ScriptReference/Animator.SetTrigger.html) 和 [ResetTrigger](../ScriptReference/Animator.ResetTrigger.html)。

以下是基于用户输入和碰撞检测而修改参数的脚本示例。



````
using UnityEngine;
using System.Collections;

public class SimplePlayer : MonoBehaviour {
	
	Animator animator;
    
	// 使用此函数进行初始化
	void Start () {
		animator = GetComponent<Animator>();
	}
	
	// 每帧调用一次 Update
	void Update () {
		float h = Input.GetAxis("Horizontal");
		float v = Input.GetAxis("Vertical");
		bool fire = Input.GetButtonDown("Fire1");

		animator.SetFloat("Forward",v);
		animator.SetFloat("Strafe",h);
		animator.SetBool("Fire", fire);
	}

	void OnCollisionEnter(Collision col) {
		if (col.gameObject.CompareTag("Enemy"))
		{
			animator.SetTrigger("Die");
		}
	}
}


````

