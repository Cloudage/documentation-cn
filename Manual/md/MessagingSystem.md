
消息系统
=============
新的 UI 系统使用一种消息系统来取代 SendMessage。该系统是纯 C# 系统，旨在解决 SendMessage 存在的一些问题。该系统使用可在 MonoBehaviour 上实现的自定义接口来指示组件能够从消息系统接收回调。调用时会指定目标游戏对象；该调用将在游戏对象的所有（实现了指定接口以据此发出该调用的）组件上发出。借助消息系统可传递自定义用户数据，并可指定事件应在游戏对象层级视图中传播的距离：应该仅为指定的游戏对象执行，还是应该在子对象和父对象上也执行。除此之外，消息框架还提供 helper 函数来搜索和查找实现了给定消息接口的游戏对象。

消息系统是通用型系统，不仅可用于 UI 系统，还可用于一般游戏代码。添加自定义消息事件相对简单，借助 UI 系统用于所有事件处理的相同框架即可实现。

如何定义自定义消息？
-------

如果希望定义自定义消息，此过程相对简单。在 UnityEngine.EventSystems 命名空间中，有一个名为“IEventSystemHandler”的基本接口。从此接口扩展的任何内容都可以视为通过消息系统接收事件的目标。

````
public interface ICustomMessageTarget : IEventSystemHandler
{
	// 可通过消息系统调用的函数
	void Message1();
	void Message2();
} 
````

一旦定义了此接口，即可由 MonoBehaviour 实现。此接口实现后，定义了在针对此 MonoBehaviour 游戏对象发出指定消息时将会执行的函数。

````
public class CustomMessageTarget : MonoBehaviour, ICustomMessageTarget
{
	public void Message1()
	{
		Debug.Log ("Message 1 received");
	}

	public void Message2()
	{
		Debug.Log ("Message 2 received");
	}
}
````

现在有了可接收消息的脚本之后，我们需要发出消息。通常，此消息用于响应发生的某个松散耦合事件。例如，在 UI 系统中，我们为 PointerEnter 和 PointerExit 等事件发出事件，还有为了响应用户在应用程序中的输入而发生的各种其他事件。

要发送消息，可使用一个静态 helper 类来执行此操作。在参数方面，需要消息的目标对象、一些特定于用户的数据以及一个映射到所需目标消息接口中特定函数的仿函数 (functor)。

````
ExecuteEvents.Execute<ICustomMessageTarget>(target, null, (x,y)=>x.Message1());
````

此代码将在游戏对象目标上实现了 ICustomMessageTarget 接口的所有组件上执行 Message1 函数。ExecuteEvents 类的脚本文档中介绍了执行函数的其他形式，例如在子对象或父对象中执行。
