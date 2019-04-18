# 脚本序列化错误

序列化是将数据结构或对象状态转换为 Unity 可存储并在以后可重构的格式的自动过程。（请参阅有关[脚本序列化](script-Serialization.html)的文档以了解更多信息。）

在某些情况下，脚本序列化可能会导致错误。下面列出了其中一些错误的修复方法。

#### 从构造函数或字段初始化函数调用 Unity Scripting API

在 [MonoBehaviour](../ScriptReference/MonoBehaviour.html) 构造函数或字段初始化函数中调用诸如 [GameObject.Find](../ScriptReference/GameObject.Find.html) 之类的脚本 API 会触发错误：“Find is not allowed to be called from a MonoBehaviour constructor (or instance field initializer), call in in Awake or Start instead.”

解决此问题的方法是在 [MonoBehaviour.Start](../ScriptReference/MonoBehaviour.Start.html) 中而不是在构造函数中调用脚本 API。

#### 在反序列化期间调用 Unity Scripting API

从标有 `System.Serializable` 的类的构造函数中调用诸如 [GameObject.Find](../ScriptReference/GameObject.Find.html) 之类的脚本 API 会触发错误：“Find is not allowed to be called during serialization, call it from Awake or Start instead.”

要解决此问题，请编辑代码，确保不会在任何序列化对象的构造函数中调用任何脚本 API。

#### 线程安全的 Unity Scripting API

大多数脚本 API 都受上面列出的限制所影响。只有部分 Unity Scripting API 免受影响，可从任何位置接受调用。它们如下：

* [Debug.Log](../ScriptReference/Debug.Log.html)

* [Mathf](../ScriptReference/Mathf.html) 函数

* 简单的独立结构；例如，像 [Vector3](../ScriptReference/Vector3.html) 和 [Quaternion](../ScriptReference/Quaternion.html) 这样的数学结构

为了降低序列化过程中出错的风险，只能调用不需要在 Unity 本身中获取或设置数据的独立 API 方法。仅在没有其他选择的情况下才调用它们。
<br/><br/>

----

<span class="page-edit">• 2017-05-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span><br/>
