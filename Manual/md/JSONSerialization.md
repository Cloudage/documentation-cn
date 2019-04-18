#JSON 序列化

JSON 序列化功能将对象转换为 [JSON](http://www.json.org) 格式或从 JSON 格式转换对象。此功能在与 Web 服务交互时很有用，也可直接用于轻松地将数据打包和解压为基于文本的格式。

有关 JsonUtility 类的信息，请参阅 Unity ScriptRef [JsonUtility](../ScriptReference/JsonUtility.html) 页面。

## 简单用法

JSON 序列化功能围绕“结构化”JSON 的概念而构建，因此可通过创建类或结构来描述将在 JSON 数据中存储的变量。例如：

````
[Serializable]
public class MyClass
{
	public int level;
	public float timeElapsed;
	public string playerName;
}
````

此代码段定义了一个包含三个变量（__level__、__timeElapsed__ 和 __playerName__）的普通 C# 类，并将该类标记为 Serializable，这是使用 JSON 序列化程序所必需的。然后，可按如下方式创建该类的实例：

````
MyClass myObject = new MyClass();
myObject.level = 1;
myObject.timeElapsed = 47.5f;
myObject.playerName = "Dr Charles Francis";
````

并使用 ``JsonUtility.ToJson`` 将其序列化为 JSON 格式：

````
string json = JsonUtility.ToJson(myObject);
````

因此将产生包含字符串的 ``json`` 变量：

````
{"level":1,"timeElapsed":47.5,"playerName":"Dr Charles Francis"}
````

要将 JSON 转换回对象，请使用 ``JsonUtility.FromJson``：

````
myObject = JsonUtility.FromJson<MyClass>(json);
````

这将创建一个新的 ``MyClass`` 实例，并使用 JSON 数据设置该实例的值。如果 JSON 数据包含的值未映射到 ``MyClass`` 中的字段，则将忽略这些值，而如果 JSON 数据缺少 ``MyClass`` 字段的值，则这些字段将保留返回对象中的构造值。

JSON 序列化程序当前不支持使用“非结构化”JSON（即，以键/值对的任意树形式导航和编辑 JSON）。如果需要这样做，应采用功能更全面的 JSON 库。

## 用 JSON 覆盖对象

也可以获取 JSON 数据并在已经创建的对象“之上”反序列化该数据，从而覆盖已存在的数据：

````
JsonUtility.FromJsonOverwrite(json, myObject);
````

在 JSON 中不包含值的对象上的任何字段都将保持不变。此方法允许通过重用先前创建的对象将分配保持在最低限度，并允许故意用仅包含一小部分字段的 JSON 来覆盖对象以便“修补”对象。

请注意，JSON 序列化程序 API 支持 ``MonoBehaviour`` 和 ``ScriptableObject`` 子类以及普通结构/类。但是，将 JSON 反序列化为 ``MonoBehaviour`` 或 ``ScriptableObject`` 的子类时，_必须_使用 FromJsonOverwrite；FromJson 不受支持并将抛出异常。

##支持的类型
该 API 支持任何 ``MonoBehaviour`` 子类、``ScriptableObject`` 子类或者带有 ``[Serializable]`` 属性的普通类/结构。传入的对象将被送入标准 Unity 序列化程序进行处理，因此需要遵循与在 Inspector 中相同的规则和限制；只序列化字段，不支持类似 ``Dictionary&lt;&gt;`` 的类型。

目前不支持将其他类型直接传递给 API，例如原始类型或数组。现在需要将这些类型包裹在某种 ``class`` 或 ``struct`` 中。

在 Editor 中且仅在 Editor 中有一个并行 API：``EditorJsonUtility``，允许将 ``UnityEngine.Object`` 派生的任何类型与 JSON 进行互相序列化。这样生成的 JSON 将包含与对象的 YAML 表示相同的数据。

##性能
基准测试表明，``JsonUtility`` 要比流行的 .NET JSON 解决方案快得多（尽管功能比其中一些解决方案更少）。

GC 内存使用量为最低量：

* ``ToJson()`` 仅为返回的字符串分配 GC 内存。
* ``FromJson()`` 仅为返回的对象以及所需的所有子对象分配 GC 内存（例如，如果对包含数组的对象进行反序列化，则将为该数组分配 GC 内存）。
* ``FromJsonOverwrite()`` 仅根据需要为写入的字段（例如字符串和数组）分配 GC 内存。如果 JSON 覆盖的所有字段都是值类型，则不应分配任何 GC 内存。

**允许**使用后台线程中的 JsonUtility API。与任何多线程代码一样，在一个线程上序列化/反序列化对象时，应注意不要在另一个线程上访问或更改该对象。

##控制 ToJson() 的输出

``ToJson`` 支持完美打印 JSON 输出。此功能默认为关闭状态，但可通过传递 ``true`` 作为第二个参数来打开此功能。

可使用 ``[NonSerialized]`` 属性从输出中省略字段。

##未提前知道类型的情况下使用 FromJson()
将 JSON 反序列化为包含“公共”字段的类或结构，然后使用这些字段的值来计算出您想要的实际类型。然后第二次反序列化为该类型。
