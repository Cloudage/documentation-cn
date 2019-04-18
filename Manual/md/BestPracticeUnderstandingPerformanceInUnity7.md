# 一般优化

有多少原因导致性能问题，就有多少种不同的方式来优化代码。通常，强烈建议开发者在尝试应用 CPU 优化之前对其应用程序进行性能分析。不过，还是存在几种普遍适用的简易 CPU 优化方式。

## 按 ID 寻址属性

Unity 不使用字符串名称对 Animator、Material 和 Shader 属性进行内部寻址。为了加快速度，所有属性名称都经过哈希处理为属性 ID，实际上正是这些 ID 用于寻址属性。

因此，每当在 Animator、Material 或 Shader 上使用 *Set* 或 *Get* 方法时，请使用整数值方法而非字符串值方法。字符串方法只执行字符串哈希处理，然后将经过哈希处理的 ID 转发给整数值方法。

从字符串哈希创建的属性 ID 在单次运行过程中是不变的。它们最简单的用法是为每个属性名称声明一个静态只读整数变量，然后使用整数变量代替字符串。启动期间将自动进行初始化，无需其他初始化代码。

[Animator.StringToHash](../ScriptReference/Animator.StringToHash.html) 是用于 Animator 属性名称的对应 API，[Shader.PropertyToID](../ScriptReference/Shader.PropertyToID.html) 是用于 Material 和 Shader 属性名称的对应 API。

## 使用非分配物理 API

在 Unity 5.3 及更高版本中，引入了所有物理查询 API 的非分配版本。将 [RaycastAll](../ScriptReference/Physics.RaycastAll.html) 调用替换为 [RaycastNonAlloc](../ScriptReference/Physics.RaycastNonAlloc.html)，将 [SphereCastAll](../ScriptReference/Physics.SphereCastAll.html) 调用替换为 [SphereCastNonAlloc](../ScriptReference/Physics.SphereCastNonAlloc.html)，以此类推。对于 2D 应用程序，也存在所有 Physics2D 查询 API 的非分配版本。

## 与 UnityEngine.Object 子类进行 Null 比较

Mono 和 IL2CPP 运行时以特定方式处理从 [UnityEngine.Object](../ScriptReference/Object.html) 派生的类的实例。在实例上调用方法实际上是调用引擎代码，此过程必须执行查找和验证以便将脚本引用转换为对原生代码的引用。将此类型变量与 Null 进行比较的成本虽然低，但远高于与纯 C# 类进行比较的成本。因此，请避免在紧凑循环中或每帧运行的代码中进行此类 Null 比较。

### 矢量和四元数数学以及运算顺序

对于位于紧凑循环中的矢量和四元数运算，请记住整数数学比浮点数学更快，而浮点数学比矢量、矩阵或四元数运算更快。


因此，每当交换或关联算术允许时，请尝试最小化单个数学运算的成本：

```

Vector3 x;

int a, b;

// 效率较低：产生两次矢量乘法

Vector3 slow = a * x * b;

// 效率较高：一次整数乘法、一次矢量乘法

Vector3 fast = a * b * x;

```

## 内置 ColorUtility

对于必须在 HTML 格式的颜色字符串 (`#RRGGBBAA`) 与 Unity 的原生 `Color` 及 `Color32` 格式之间进行转换的应用程序来说，使用来自 Unify Community 的脚本是很常见的做法。由于需要进行字符串操作，此脚本不但速度很慢，而且会导致大量内存分配。

从 Unity 5 开始，有一个内置 [ColorUtility](../ScriptReference/ColorUtility.html) API 可以有效执行此类转换。应优先使用内置 API。

## Find 和 FindObjectOfType

一般来说，最好完全避免在生产代码中使用 `Object.Find` 和 `Object.FindObjectOfType`。由于此类 API 要求 Unity 遍历内存中的所有游戏对象和组件，因此它们会随着项目规模的扩大而产生性能问题。

在单例对象的访问器对上述规则来说是个例外。全局管理器对象往往会暴露“instance”属性，并且通常在 getter 中存在 `FindObjectOfType` 调用以便检测单例预先存在的实例：

```

class SomeSingleton {

	private SomeSingleton _instance;

	public SomeSingleton Instance {

		get {

			if(_instance == null) { 

				_instance =

					FindObjectOfType<SomeSingleton>(); 

			}

			if(_instnace == null) { 

				_instance = CreateSomeSingleton();

			}

			return _instance;

		}

	}

}

```

虽然这种模式通常是可以接受的，但必须注意检查代码并确保调用访问器时场景中不存在单例对象。如果 getter 没有自动创建缺失单例的实例，那么寻找单例的代码经常会重复调用 `FindObjectOfType`（通常每帧多次发生）并且会对性能产生不良影响。

### 摄像机定位器

在内部，Unity 的 `Camera.main` 属性会调用 `Object.FindObjectWithTag`（这是 `Object.FindObject` 的一个专用变体）。访问此属性并不比调用 `Object.FindObjectOfType` 更高效。如果代码必须寻址主摄像机，强烈建议您执行以下两项操作之一：

* 访问 `Start` 或 ` OnEnable` 回调中的 `Camera.main`，并缓存所产生的引用。

* 构造一个可提供或添加活动摄像机引用的 `Camera Manager` 类。

## 调试代码和 [conditional] 属性

`UnityEngine.Debug` 日志记录 API 并未从非开发版中剥离出去，如果被调用，则会写入日志。由于大多数开发者不打算在非开发版中写入调试信息，因此建议在自定义方法中打包仅用于开发用途的日志记录调用，如下所示：

```

    public static class Logger {

            [Conditional("ENABLE_LOGS")]

            public static void Debug(string logMsg) {

    		    UnityEngine.Debug.Log(logMsg);

            }

        }

```

通过使用 [Conditional] 属性来修饰这些方法，Conditional 属性所使用的一个或多个定义将决定被修饰的方法是否包含在已编译的源代码中。

如果传递给 Conditional 属性的任何定义均未被定义，则会被修饰的方法*以及*对被修饰方法的所有调用都会在编译中剔除。实际效果与包裹在 `#if … #endif` 预处理器代码块中的方法以及对该方法的所有调用的处理情况相同。

有关 `Conditional` 属性的更多信息，请参阅 MSDN 网站：[msdn.microsoft.com](https://msdn.microsoft.com/en-us/library/4xssyw96(v=vs.90).aspx)。

