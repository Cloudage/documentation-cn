# GridBrushEditorBase

添加的所有画笔编辑器必须继承自 `GridBrushEditorBase`。`GridBrushEditorBase` 提供了一组固定 API 用于在 Palette 窗口上绘制检视面板以及在 Scene 视图上绘制辅助图标。

```
public virtual GameObject[] validTargets 
```

这将返回作为画笔有效绘制目标的游戏对象列表。此列表将显示在 Palette 窗口内的下拉选单中。重写此 API 可提供此画笔可进行交互的自定义目标列表。

```
public virtual void OnPaintInspectorGUI() 
```

这将显示一个检视面板以用于在面板 (Palette) 中编辑画笔 (Brush) 选项。使用此 API 可在 Scene 视图中进行编辑时更新画笔功能。

```
public virtual void OnSelectionInspectorGUI() 
```

这将在目标网格上选择单元格时显示检视面板。重写此 API 可显示所选单元格的自定义检视面板视图。

```
public virtual void OnPaintSceneGUI(GridLayout grid, GameObject brushTarget, BoundsInt position, GridBrushBase.Tool tool, bool executing) 
```

这用于在使用画笔绘制时在 Scene 视图上绘制额外的辅助图标。Tool 是面板中当前选择的工具。执行结果将返回是否正在特定时间使用画笔。

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
