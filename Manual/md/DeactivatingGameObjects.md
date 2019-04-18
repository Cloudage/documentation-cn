#停用游戏对象

可以通过将游戏对象标记为非活动来暂时从场景中移除此对象。可以使用脚本的 [activeSelf](../ScriptReference/GameObject-activeSelf.html) 属性或者使用 Inspector 中的激活复选框来完成此操作


![](../uploads/Main/GOActiveBox.png) 

_游戏对象的激活复选框_



##停用父游戏对象的效果

停用父对象时，停用也会覆盖其所有子对象上的 __activeSelf__ 设置，因此父级的整个层级视图将变为非活动状态。请注意，这不会更改子对象上 __activeSelf__ 属性的值，因此一旦重新激活父对象，子对象将恢复到其原始状态。这意味着无法通过读取 __activeSelf__ 属性来确定子对象当前是否在场景中处于活动状态。而应该使用 [activeInHierarchy](../ScriptReference/GameObject-activeInHierarchy.html) 属性，该属性将考虑父对象的覆盖效果。

在 Unity 4.0 中，已引入此覆盖行为。在早期版本中，有一个称为 __SetActiveRecursively__ 的函数可用于激活或停用给定父对象的子项。但是，此函数的工作方式不同：每个子对象的激活设置都已更改 - 可以关闭和打开整个层级视图，但是子对象无法“记住”自己最初所处的状态。为了避免破坏旧版代码，__SetActiveRecursively__ 已保留在 4.0 的 API 中，但不建议予以使用，而且将来可能会将其移除。在实际希望更改子项的 __activeSelf__ 设置的极少情况下，可以使用如下代码：



````
// JavaScript
function DeactivateChildren(g: GameObject, a: boolean) {
	g.activeSelf = a;
	
	for (var child: Transform in g.transform) {
		DeactivateChildren(child.gameObject, a);
	}
}


// C#
void DeactivateChildren(GameObject g, bool a) {
	g.activeSelf = a;
	
	foreach (Transform child in g.transform) {
		DeactivateChildren(child.gameObject, a);
	}
}
````
