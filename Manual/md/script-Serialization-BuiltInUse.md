# 内置序列化

Unity 的一些内置功能自动使用序列化技术。这些功能如下所述。

请参阅有关[脚本序列化](script-Serialization.html)的文档以了解更多信息。

#### 保存和加载

Unity 使用序列化技术从计算机的硬盘驱动器加载和保存[场景](CreatingScenes.html)、[资源](AssetWorkflow.html)和 [AssetBundle](AssetBundlesIntro.html)。这包括保存在您自己的脚本 API 对象（如 [MonoBehaviour](../ScriptReference/MonoBehaviour.html) 组件和 [ScriptableObject](class-ScriptableObject.html)）中的数据。

这发生在 Editor 的 Play 模式和 Edit 模式中。

#### Inspector 窗口

在 [Inspector 窗口](UsingTheInspector.html)中查看或更改游戏对象的组件字段的值时，Unity 会序列化此数据，然后在 Inspector 窗口中显示数据。Inspector 窗口在显示字段值时不与 Unity Scripting API 通信。如果在脚本中使用属性，则在 Inspector 窗口中查看或更改值时，绝不会调用任何属性 getter 和 setter，因为 Unity 会直接序列化 Inspector 窗口字段。

#### 在 Unity Editor 中重新加载脚本

更改并保存脚本时，Unity 会重新加载所有当前加载的脚本数据。它首先将所有可序列化变量存储在所有加载的脚本中，并在加载脚本后恢复它们。重新加载脚本后，所有不可序列化的数据都将丢失。

这会影响所有 Editor 窗口以及项目中的所有 MonoBehaviour。与 Unity 中的其他序列化情况不同，私有字段在重新加载时会默认序列化，即使它们没有“SerializeField”属性也是如此。

#### 预制件

在序列化背景下，[预制件](Prefabs.html)是一个或多个[游戏对象](GameObjects.html)和[组件](Components.html)的序列化的数据。预制件实例包含对预制件源及其一系列修改的引用。这些修改是 Unity 为了创建特定预制件实例而需要对预制件源进行的操作。

只有在 Unity Editor 中编辑项目时，预制件实例才存在。在项目构建期间，Unity Editor 从两组序列化数据（预制件源和预制件实例的修改）进行游戏对象的实例化。

#### 实例化

对[场景](CreatingScenes.html)中存在的任何对象（例如[预制件](Prefabs.html)或[游戏对象](GameObjects.html)）调用 [Instantiate](../ScriptReference/Object.Instantiate.html) 时，Unity 都会对其进行序列化。在运行时和在 Editor 中都是如此。从 [UnityEngine.Object](../ScriptReference/Object.html) 派生的一切均可序列化。

Unity 随后创建一个新的游戏对象并将数据反序列化到新的游戏对象上。接下来，Unity 在另一个不同的变体中运行相同的序列化代码，以报告正在引用的其他 `UnityEngine.Objects`。它会检查所有引用的 `UnityEngine.Objects` 以查看它们是否是要实例化的数据的一部分。如果引用指向“外部”，例如纹理，则 Unity 会保持该引用不变。如果引用指向“内部”，例如子游戏对象，则 Unity 会修补对相应副本的引用。

#### 卸载未使用的资源

`Resource.GarbageCollectSharedAssets()` 是本机 Unity 垃圾回收器，与标准 C# 垃圾回收器执行不同的功能。该垃圾回收器在您加载场景之后运行，检查不再引用的对象（如纹理），并安全卸载它们。本机 Unity 垃圾回收器在一个变体中运行序列化程序，而在此变体中，对象会报告对外部 `UnityEngine.Objects` 的所有引用。通过此方式即可将一个场景使用过的纹理在下一个场景中卸载。
<br/><br/>

----

<span class="page-edit">• 2017-05-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span><br/>
