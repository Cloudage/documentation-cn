# 可编程画笔

## 创建可编程画笔

创建一个继承自 `GridBrushBase`（或 `GridBrushBase` 的任何有用子类，如 `GridBrush`）的新类。重写新的 `Brush` 类所需的所有方法。以下是将要重写的常用方法：

* `Paint` 允许画笔将项添加到目标网格上
* `Erase` 允许画笔从目标网格上移除项
* `FloodFill` 允许画笔将项填充到目标网格上
* `Rotate` 旋转画笔中设置的项。
* `Flip` 翻转画笔中设置的项。

使用 `ScriptableObject.CreateInstance<(Your Brush Class>()` 创建该新类的实例。可通过调用 `AssetDatabase.CreateAsset()` 将此新实例转换为 Editor 中的资源以便重复使用。

还可以为画笔创建自定义编辑器。这与脚本化对象的自定义编辑器的工作方式相同。以下是在创建自定义编辑器时要重写的主要方法：

* 可重写 `OnPaintInspectorGUI` 以便在选择画笔时在面板上显示检视面板，从而在绘制时提供其他行为。
* 还可以重写 `OnPaintSceneGUI` 以便在 `SceneView` 上绘制时添加其他行为。
* 还可以重写 `validTargets` 以便获得画笔可进行交互的自定义目标列表。此目标列表将显示为 Palette 窗口内的下拉选单。

创建可编程画笔后，画笔将列在 Palette 窗口的 __Brushes 下拉选单__中。默认情况下，可编程画笔脚本的实例将经过实例化并存储在项目的 _Library_ 文件夹中。对画笔属性的任何修改都会存储在该实例中。如果希望该画笔有多个具备不同属性的副本，可在项目中将画笔实例化为资源。这些画笔资源将在 Brush 下拉选单中单独列出。

可将一个 `CustomGridBrush` 属性添加到可编程画笔类。这样就能在 Palette 窗口中配置画笔的行为。`CustomGridBrush` 属性具有以下属性：

* `HideAssetInstances` - 将此属性设置为 true 会在 Palette 窗口中隐藏所创建的画笔资源的所有副本。如果仅希望默认实例显示在 Palette 窗口的 Brush 下拉选单中，请设置此属性。
* `HideDefaultInstances` - 将此属性设置为 true 会在 Palette 窗口中隐藏画笔的默认实例。如果仅希望创建的资源显示在 Palette 窗口的 Brush 下拉选单中，请设置此属性。
* `DefaultBrush` - 将此属性设置为 true 会将画笔的默认实例设置为项目中的默认画笔。如此一来，每当项目启动时，此画笔始终是默认选择的画笔。仅应将一个可编程画笔设置为默认画笔！
* `DefaultName` - 设置此属性会使 Brush 下拉选单使用此属性作为画笔的名称而不是画笔类的名称。

请记住保存项目以便确保对新画笔资源进行保存！

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
