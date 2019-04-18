升级到 Unity 4.0
===================================



游戏对象活动状态
-----------------------


Unity 4.0 更改了游戏对象活动状态的处理方式。游戏对象的活动状态现在会由子游戏对象继承，因此任何非活动游戏对象也会导致其子项处于非活动状态。我们相信新行为比旧行为更合理，而且应该始终如此。此外，即将推出的新 GUI 系统在很大程度上取决于新的 4.0 行为，没有它就不可能实现。不幸的是，这可能需要执行一些工作来修复现有项目以便使用新的 Unity 4.0 行为，更改如下：

###旧行为：


* 游戏对象是否处于活动状态由其 __.active__ 属性定义。
* 可通过检查 __.active__ 属性来查询和设置此状态。
* 游戏对象的活动状态对子游戏对象的活动状态没有影响。如果要激活或停用游戏对象及其所有子项，需要调用 __GameObject.SetActiveRecursively__。
* 对游戏对象使用 __SetActiveRecursively__ 时，任何子游戏对象的先前活动状态都将丢失。使用 __SetActiveRecursively__ 停用再激活游戏对象及其所有子项时，任何在调用 __SetActiveRecursively__ 之前处于非活动状态的子项都将变为活动状态，而如果要将子项恢复到原来状态，必须手动跟踪子项的活动状态。
* 预制件不能包含任何活动状态，并在预制件实例化后始终处于活动状态。

###新行为：


* 游戏对象是否处于活动状态由其自己的 __.activeSelf__ 属性及其所有父项的该属性定义。如果游戏对象自己的 __.activeSelf__ 属性及其所有父项的该属性为 __true__，则该游戏对象处于活动状态。如果任何这些对象的属性为 __false__，则该游戏对象为非活动状态。
* 可使用 __.activeInHierarchy__ 属性来查询此状态。
* 可通过调用 __GameObject.SetActive__ 来更改游戏对象的 __.activeSelf__ 状态。对以前处于活动状态的游戏对象调用 __SetActive (false)__ 时，将停用游戏对象及其所有子项。对以前处于非活动状态的游戏对象调用 __SetActive (true)__ 时，如果该游戏对象的所有父项均为活动状态，则将激活该游戏对象。当子项的所有父项都是活动状态时（即它们所有的父项都将 __.activeSelf__ 设置为 __true__ 时），子项将被激活。
* 这意味着不再需要 __SetActiveRecursively__，因为活动状态将从父项继承。这也意味着，通过调用 __SetActive__ 来停用和激活层级视图中的一部分时，将保留所有子游戏对象的先前活动状态。
* 预制件可以包含活动状态，该状态在预制件实例化时保留。

###示例：

假如有三个游戏对象：A、B 和 C，其中 B 和 C 是 A 的子项。

* 通过调用 __C.SetActive(false)__ 来停用 C。
* 现在，__A.activeInHierarchy == true__，__B.activeInHierarchy == true__，而 __C.activeInHierarchy == false__。
* 同样，__A.activeSelf == true__，__B.activeSelf == true__，而 __C.activeSelf == false__。
* 现在通过调用 __A.SetActive(false)__ 来停用父项 A。
* 现在，__A.activeInHierarchy == false__，__B.activeInHierarchy == false__，而 __C.activeInHierarchy == false__。
* 同样，__A.activeSelf == false__，__B.activeSelf == true__，而 __C.activeSelf == false__。
* 现在通过调用 __A.SetActive(true)__ 再次激活父项 A。
* 现在，恢复为 __A.activeInHierarchy == true__，__B.activeInHierarchy == true__，而 __C.activeInHierarchy == false__。
* 同样，__A.activeSelf == true__，__B.activeSelf == true__，而 __C.activeSelf == false__。

###Editor 中的新活动状态

为了使这些更改可视化，在 Unity 4.0 Editor 中，任何非活动游戏对象（由于其自己的 __.activeSelf__ 属性或者其中一个父项的该属性设置为 __false__）将在层级视图中显示为灰色，并在 Inspector 中具有灰色图标。游戏对象自己的 __.activeSelf__ 属性由其活动状态复选框反映，无论父项状态如何都可以切换该属性（但只有在所有父项都处于活动状态时才会激活该游戏对象）。

###对现有项目的影响：


* 为了让您了解此更改在代码方面可能带来的影响，__GameObject.active__ 属性和 __GameObject.SetActiveRecursively()__ 函数已弃用。
* 然而，它们仍然有效。读取 __GameObject.active__ 的值等同于读取 __GameObject.activeInHierarchy__，设置 __GameObject.active__ 等同于调用 __GameObject.SetActive()__。调用 __GameObject.SetActiveRecursively()__ 等同于对游戏对象及其所有子项调用 __GameObject.SetActive()__。
* 通过将场景中任何游戏对象的 __selfActive__ 属性设置为先前的 __active__ 属性，可以导入 3.5 版的现有场景。
* 因此，对于从早期版本的 Unity 导入的任何项目，只要不依赖于非活动游戏对象的活动子项（Unity 4.0 中不再支持），应该仍能按预期工作（但会有编译器警告）。
* 如果项目依赖于非活动游戏对象的活动子项，需要将逻辑更改为 Unity 4.0 中适用的模型。



对资源处理管线的更改
----------------------------------------


在 4.0 版的开发过程中，我们的资源导入管线在内部做了一些重要改变，旨在提高性能、内存使用率和确定性。在大多数情况下，这些更改对用户没有影响，但有一个例外：资源中的对象直到导入管线的最后阶段才会持久化，并且所有以前导入的资源版本都将被完全替换。

第一点意味着在后期处理期间无法获得对资源中的对象的正确引用，第二点意味着如果在后期处理期间使用对先前导入资源版本的引用来存储修改，这些修改将丢失。

###因为尚未持久化而导致引用丢失的示例
请参考下面这个小例子：



````
public class ModelPostprocessor : AssetPostprocessor
{
    public void OnPostprocessModel(GameObject go)
    {
        PrefabUtility.CreatePrefab("Prefabs/" + go.name, go);
    }
}


````

在 Unity 3.5 中，此代码创建的预制件将包含对网格的所有正确引用等等，这是因为所有网格都已持久化，但由于 Unity 4.0 中的情况并非如此，相同的后期处理程序创建的预制件中所有对网格的引用都会消失，这是因为 Unity 4.0 还不知道如何解析对原始模型预制件中对象的引用。要将模型预制件正确复制到预制件，应使用 __OnPostProcessAllAssets__ 来处理所有导入的资源，查找模型预制件，并按如上所述创建新预制件。


###对丢弃的先前导入资源的引用示例
第二个示例稍微复杂一点，但实际上是 3.5 版中正常但在 4.0 版中有问题的用例。这是一个引用了网格的简单 __ScriptableObject__。



````
public class Referencer : ScriptableObject
{
    public Mesh myMesh;	
} 


````

我们使用这个 __ScriptableObject__ 来创建一个资源并使该资源引用模型中的网格，然后在后期处理程序中使用该引用并为其指定一个不同的名称，最终结果是当我们重新导入模型时，网格的名称将是后期处理程序确定的名称。



````
public class Postprocess : AssetPostprocessor
{
	public void OnPostprocessModel(GameObject go)
	{
		Referencer myRef = (Referencer)AssetDatabase.LoadAssetAtPath("Assets/MyRef.asset", typeof(Referencer));
		myRef.myMesh.name = "AwesomeMesh";
	}
}


````

此代码在 Unity 3.5 中可按预期工作，但在 Unity 4.0 中，已经导入的模型将被完全替换，因此更改先前导入的网格的名称将不起作用。这里的解决方案是通过其他方式找到网格，然后更改其名称。最重要的是要注意，在 Unity 4.0 中，应当只修改提供给后期处理程序的指定输入，而不是依赖于该资源的先前导入版本。



网格读取/写入选项
----------------------


Unity 4.0 在[模型导入设置](class-FBXImporter.html)中添加了“Read/Write Enabled”选项。关闭此选项可以节省内存，因为 Unity 可以在游戏中卸载网格数据副本。

但是，如果在运行时使用非一致比例缩放或实例化网格，可能必须在这些网格的导入设置中启用“Read/Write Enabled”。原因是非一致缩放需要将网格数据保存在内存中。通常我们在构建时会检测到这一点，但在运行时缩放或实例化网格时，需要手动设置进行此设置。否则，这些网格可能无法在游戏构建中正确渲染。

网格优化
-----------------


Unity 4.0 中的 Model Importer 在网格优化方面进一步改善。Unity 4.0 的 Model Importer 中的“Mesh Optimization”复选框现在默认为启用状态，并会对网格中的顶点重新排序以获得最佳性能。项目中可能会有一些后期处理代码或效果，这些代码或效果取决于网格的顶点顺序，并可能会因此更改而出现问题。在这种情况下，请关闭 Mesh Importer 中的“Mesh Optimization”。尤其是在使用 SkinnedCloth 组件的情况下，网格优化将导致顶点权重映射发生变化。因此，如果在从 3.5 导入的项目中使用 SkinnedCloth，需要为受影响的网格关闭“Mesh Optimization”，或者重新配置顶点权重来匹配新的顶点顺序。

移动端输入
------------


在 Unity 4.0 中，移动端传感器输入在平台之间具有更好的一致性，这意味着可以在处理移动平台上的典型输入时编写更少的代码。现在加速度和陀螺仪输入将在 iOS 和 Android 平台上以相同的方式追踪屏幕方向。为了利用此更改带来的优势，应在处理加速度和陀螺仪输入时重构输入代码并删除关于平台和屏幕方向的代码。通过将 __Input.compensateSensors__ 设置为 false，仍然能在 iOS 上获得以前的行为。
