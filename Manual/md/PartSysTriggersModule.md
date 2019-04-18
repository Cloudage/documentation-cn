# Triggers 模块

粒子系统能够在与场景中的一个或多个碰撞体交互时触发回调。当粒子进入或离开碰撞体时，或在粒子位于碰撞体内部或外部时，可触发回调。

您可以使用回调作为粒子进入碰撞体时销毁粒子（例如防止雨滴穿透屋顶）的简单方法，也可使用回调来修改任何或所有粒子的属性。

Triggers 模块还提供 __Kill__ 选项（用于自动移除粒子）和 __Ignore__ 选项（用于忽略碰撞事件），如下所示。

![粒子系统 Triggers 模块](../uploads/Main/PartSysTriggersModule.png)

要使用该模块，首先添加需要创建触发器的碰撞体，然后选择要使用的事件。

可选择在粒子处于以下状态时触发事件：

* 位于碰撞体的边界范围内
* 位于碰撞体的边界范围外
* 进入碰撞体的边界范围
* 离开碰撞体的边界范围

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Inside__ |如果希望在粒子位于碰撞体内时触发事件，请选择 Callback。如果希望在粒子位于碰撞体内时**不**触发事件，请选择 Ignore。选择 Kill 可销毁碰撞体内的粒子。 |
|__Outside__ |如果希望在粒子位于碰撞体外时触发事件，请选择 Callback。如果希望在粒子位于碰撞体外时**不**触发事件，请选择 Ignore。选择 Kill 可销毁碰撞体外的粒子。 |
|__Enter__ |如果希望在粒子进入碰撞体时触发事件，请选择 Callback。如果希望在粒子进入碰撞体时**不**触发事件，请选择 Ignore。选择 Kill 可在粒子进入碰撞体时销毁粒子。 |
|__Exit__ |如果希望在粒子离开碰撞体时触发事件，请选择 Callback。如果希望在粒子离开碰撞体时**不**触发事件，请选择 Ignore。选择 Kill 可在粒子离开碰撞体时销毁粒子。 |
|__Radius Scale__| 此参数用于设置粒子的碰撞体边界，让事件在粒子接触碰撞体之前或之后出现。例如，您可能希望粒子在弹回之前看起来稍微穿透碰撞体对象的表面，在这种情况下便可将 Radius Scale 设置为略小于 1。请注意，当事件实际触发时，此设置不会更改，但可以延迟或提前触发器的视觉效果。<br/>- 输入 1 表示事件在粒子接触碰撞体时出现 <br/>- 输入小于 1 的值表示触发器在粒子穿透碰撞体之前出现 <br/>- 输入大于 1 的值表示触发器在粒子穿透碰撞体之后出现|
|__Visualize Bounds__ | 此设置可在 Editor 窗口中显示粒子的碰撞体边界。|

在回调内部，应使用 [ParticlePhysicsExtensions.GetTriggerParticles()](../ScriptReference/ParticlePhysicsExtensions.GetTriggerParticles.html)（连同要指定的 [ParticleSystemTriggerEventType](../ScriptReference/ParticleSystemTriggerEventType.html)）确定哪些粒子符合哪个标准。

下面的示例会导致粒子在进入碰撞体时变为红色，然后在离开碰撞体的边界时变为绿色。

````
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

[ExecuteInEditMode]
public class TriggerScript : MonoBehaviour
{
	ParticleSystem ps;

	// 这些列表用于包含与每帧的触发条件
	// 匹配的粒子。
	List<ParticleSystem.Particle> enter = new List<ParticleSystem.Particle>();
	List<ParticleSystem.Particle> exit = new List<ParticleSystem.Particle>();

	void OnEnable()
	{
    	ps = GetComponent<ParticleSystem>();
	}

	void OnParticleTrigger()
	{
    	// 获取与此帧的触发条件匹配的粒子
    	int numEnter = ps.GetTriggerParticles(ParticleSystemTriggerEventType.Enter, enter);
    	int numExit = ps.GetTriggerParticles(ParticleSystemTriggerEventType.Exit, exit);

    	// iterate through the particles which entered the trigger and make them red
    	for (int i = 0; i < numEnter; i++)
    	{
        	ParticleSystem.Particle p = enter[i];
        	p.startColor = new Color32(255, 0, 0, 255);
        	enter[i] = p;
    	}

    	// 迭代离开触发器的粒子并使它们变绿
    	for (int i = 0; i < numExit; i++)
    	{
        	ParticleSystem.Particle p = exit[i];
        	p.startColor = new Color32(0, 255, 0, 255);
        	exit[i] = p;
    	}

    	// 将修改后的粒子重新分配回粒子系统
    	ps.SetTriggerParticles(ParticleSystemTriggerEventType.Enter, enter);
    	ps.SetTriggerParticles(ParticleSystemTriggerEventType.Exit, exit);
	}
}

````

结果如下图所示：

![Editor 视图](../uploads/Main/PartSysTriggersModule-ExampleEditorView.jpg)

![Game 视图](../uploads/Main/PartSysTriggersModule-ExampleGameView.jpg)

