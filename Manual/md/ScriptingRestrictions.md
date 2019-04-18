# 脚本限制

我们致力于在 Unity 支持的所有平台之间提供通用的脚本 API 和体验。但是，有些平台存在固有的限制。为帮助您了解这些限制并支持跨平台代码，下表描述了每个平台和脚本后端适用的限制：

## .NET 4.x 等效脚本运行时

| **_平台（脚本后端）_** | **_提前编译_** | **_无线程_** | **_.NET Core 类库子集_** |  
|:---|:---|:---|:---|
| Android (IL2CPP) | &amp;#x2714; |  |  | 
| Android (Mono) |  |  |  |
| iOS (IL2CPP) | &amp;#x2714; |  |  |
| PlayStation 4 (IL2CPP) | &amp;#x2714; |  |  |  
| PlayStation Vita (IL2CPP) | &amp;#x2714; |  |  |  
| 独立平台 (IL2CPP) | &amp;#x2714; |  |  |
| 独立平台 (Mono) |  |  |  |
| Switch (IL2CPP) | &amp;#x2714; |  |  | 
| 通用 Windows 平台 (IL2CPP) | &amp;#x2714; |  |  | 
| 通用 Windows 平台 (.NET)| |  | &amp;#x2714; |  
| WebGL (IL2CPP) | &amp;#x2714; | &amp;#x2714; |  ||
| WiiU (Mono) |  |  |  | 
| XBox One (IL2CPP) | &amp;#x2714; |  |  | 

## .NET 3.5 等效脚本运行时

| **_平台（脚本后端）_** | **_提前编译_** | **_无线程_** | **_.NET Core 类库子集_** |  
|:---|:---|:---|:---|
| Android (IL2CPP) | &amp;#x2714; |  |  | 
| Android (Mono) |  |  |  |
| iOS (IL2CPP) | &amp;#x2714; |  |  |
| PlayStation 4 (IL2CPP) | &amp;#x2714; |  |  | 
| PlayStation 4 (Mono) | &amp;#x2714; |  |  |
| PlayStation Vita (IL2CPP) | &amp;#x2714; |  |  | 
| PlayStation Vita (Mono) | &amp;#x2714; |  |  | 
| 独立平台 (IL2CPP) | &amp;#x2714; |  |  |
| 独立平台 (Mono) |  |  |  |
| Switch (IL2CPP) | &amp;#x2714; |  |  | 
| 通用 Windows 平台 (IL2CPP) | &amp;#x2714; |  |  | 
| 通用 Windows 平台 (.NET)| |  | &amp;#x2714; |  
| WebGL (IL2CPP) | &amp;#x2714; | &amp;#x2714; |  ||
| WiiU (Mono) |  |  |  | 
| XBox One (IL2CPP) | &amp;#x2714; |  |  | 
| XBox One (Mono) | &amp;#x2714; |  |  |

<a name="AOT"></a> 
## 提前编译

有些平台不允许生成运行时代码。因此，任何依赖于在目标设备上即时 (JIT) 编译的托管代码都将失败。相反，我们需要提前 (AOT) 编译所有托管代码。通常，这种区别并不重要，但在少数特定情况下，AOT 平台需要额外注意。

### System.Reflection.Emit

AOT 平台无法实现 **System.Reflection.Emit** 命名空间中的任何方法。请注意，**System.Reflection** 的其余部分是可接受的，只要编译器可以推断通过反射使用的代码需要在运行时存在。

### 序列化

AOT 平台可能会由于使用了反射而遇到序列化和反序列化问题。如果仅通过反射将某个类型或方法作为序列化或反序列化的一部分使用，则 AOT 编译器无法检测到需要为该类型或方法生成代码。

### 通用虚拟方法

通用方法要求编译器做一些额外的工作，将开发人员编写的代码扩展到设备上实际执行的代码。例如，对于具有 **int** 或 **double** 类型的 **List**，我们需要不同代码。虚拟方法在运行时而不是编译时确定行为，存在虚拟方法时，编译器可在不完全明显的地方轻松地要求从源代码生成运行时代码。

假设有以下代码，此代码在 JIT 平台上完全按预期工作（向控制台输出一次“Message value: Zero”）：

```
using UnityEngine;
using System;

public class AOTProblemExample : MonoBehaviour, IReceiver {
	public enum AnyEnum {
		Zero,
		One,
	}

	void Start() {
		// 微妙的触发器：管理器的类型*必须*是
		// IManager（而不是 Manager）才能触发 AOT 问题。
		IManager manager = new Manager();
		manager.SendMessage(this, AnyEnum.Zero);
	}

	public void OnMessage<T>(T value) {
		Debug.LogFormat("Message value: {0}", value);
	}
}

public class Manager : IManager {
	public void SendMessage<T>(IReceiver target, T value) {
		target.OnMessage(value);
	}
}

public interface IReceiver {
	void OnMessage<T>(T value);
}

public interface IManager {
	void SendMessage<T>(IReceiver target, T value);
}
```

使用 IL2CPP 脚本后端在 AOT 平台上执行此代码时，发生以下异常：

```
ExecutionEngineException: Attempting to call method 'AOTProblemExample::OnMessage<AOTProblemExample+AnyEnum>' for which no ahead of time (AOT) code was generated.
  at Manager.SendMessage[T] (IReceiver target, .T value) [0x00000] in <filename unknown>:0 
  at AOTProblemExample.Start () [0x00000] in <filename unknown>:0
```

同样，Mono 脚本后端提供以下类似的异常：

```
  ExecutionEngineException: Attempting to JIT compile method 'Manager:SendMessage<AOTProblemExample/AnyEnum> (IReceiver,AOTProblemExample/AnyEnum)' while running with --aot-only.
    at AOTProblemExample.Start () [0x00000] in <filename unknown>:0
```

AOT 编译器不会意识到自己应该为 **T** 为 **AnyEnum** 的泛型方法 **OnMessage** 生成代码，所以它直接继续往下，跳过该方法。调用该方法时，运行时无法找到要执行的正确代码，因此抛出此错误消息。

要解决像这样的 AOT 问题，我们通常可以强制编译器为我们生成适当的代码。如果我们向 **AOTProblemExample** 类添加如下所示的方法：

```
public void UsedOnlyForAOTCodeGeneration() {
	// IL2CPP 只需要这一行。
	OnMessage(AnyEnum.Zero);

	//Mono 也需要这一行。请注意，我们
	// 直接在 Manager 而不是 IManager 接口上调用。
	new Manager().SendMessage(null, AnyEnum.Zero);

	//包含一个异常，这样我们就可以确定是否调用过该方法。
	throw new InvalidOperationException("This method is used for AOT code generation only.Do not call it at runtime.");
}
```

当编译器遇到 **T** 为 **AnyEnum** 的 **OnMessage** 的显式调用时，它会生成运行时执行的正确代码。不需要调用 **UsedOnlyForAOTCodeGeneration** 方法；该方法的存在只是为了让编译器看到而已。

<a name="threads"></a> 
## 无线程

有些平台不支持使用线程，因此任何使用 **System.Threading** 命名空间的托管代码都将在运行时失败。此外，.NET 类库的某些部分存在对线程的隐式依赖。一个常用的例子是 **System.Timers.Timer** 类，它依赖于对线程的支持。

---

* <span class="page-edit">2017-11-20  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>

* <span class="page-history">删除了三星电视支持。</span>
