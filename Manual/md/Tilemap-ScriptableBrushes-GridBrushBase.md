# GridBrushBase

添加的所有画笔必须继承自 `GridBrushBase`。`GridBrushBase` 提供了一组固定 API 用于绘制。

```
public virtual void Paint(GridLayout grid, GameObject brushTarget, Vector3Int position) 
```

`Paint` 使用 `GridLayout` 网格在给定位置将数据添加到目标游戏对象 `brushTarget` 上。在网格上激活画笔并在 Palette 窗口中选择__绘画工具 (Paint Tool)__ 时会触发此 API。重写此 API 可以实现所需的绘制操作。

```
public virtual void Erase(GridLayout grid, GameObject brushTarget, Vector3Int position) 
```

`Erase` 使用 `GridLayout` 网格在给定位置删除目标游戏对象 `brushTarget` 上的数据。在网格上激活画笔并在 Palette 窗口中选择__擦除工具 (Erase Tool)__ 时会触发此 API。重写此 API 可以实现所需的擦除操作。

```
public virtual void BoxFill(GridLayout grid, GameObject brushTarget, BoundsInt position) 
```

`BoxFill` 使用 `GridLayout` 网格在给定范围内将数据添加到目标游戏对象 `brushTarget` 上。在网格上激活画笔并在 Palette 窗口中选择__框填工具 (Box Fill Tool)__ 时会触发此 API。重写此 API 可以实现所需的填充操作。

```
public virtual void FloodFill(GridLayout grid, GameObject brushTarget, Vector3Int position) 
```

`FloodFill` 使用 `GridLayout` 网格将数据添加到目标游戏对象 `brushTarget` 上，此过程从给定位置开始填充与该位置相关的所有其他可能区域。在网格上激活画笔并在 Palette 窗口中选择__灌填工具 (Flood Fill Tool)__ 时会触发此 API。重写此 API 可以实现所需的填充操作。

```
public virtual void Rotate(RotationDirection direction) 
```

`Rotate` 根据当前设置的轴心按给定方向旋转画笔中的内容。

```
public virtual void Flip(FlipAxis flip) 
```

`Flip` 根据当前设置的轴心按给定轴翻转画笔的内容。

```
public virtual void Select(GridLayout grid, GameObject brushTarget, BoundsInt position) 
```

`Select` 使用 `GridLayout` 网格根据给定范围来标记目标游戏对象 `brushTarget` 上的边界。因此允许根据所选边界查看信息，并使用__移动工具 (Move Tool)__ 移动选择的对象。在网格上激活画笔并在 Palette 窗口中选择“选择工具”时会触发此 API。重写此 API 可以在从目标进行选择时实现所需的操作。

```
public virtual void Pick(GridLayout grid, GameObject brushTarget, BoundsInt position, Vector3Int pivot) 
```

`Pick` 使用 `GridLayout` 网格从给定范围和轴心位置拉取目标游戏对象 `brushTarget` 的数据，并用该数据填充画笔。在网格上激活画笔并在 Palette 窗口中选择__选取工具 (Pick Tool)__ 时会触发此 API。重写此 API 可以在从目标进行选取时实现所需的操作。

```
public virtual void Move(GridLayout grid, GameObject brushTarget, BoundsInt from, BoundsInt to) 
```

`Move` 使用 `GridLayout` 网格从给定起始位置到给定结束位置标记从目标游戏对象 `brushTarget` 进行的移动。重写此 API 可以在从目标进行移动时实现所需的操作。在网格上激活画笔并在 Palette 窗口中选择__移动工具 (Move Tool)__ 并执行 Move (`MouseDrag`) 时会触发此 API。通常，这是从画笔执行 `Move` 操作时的任何行为。

```
public virtual void MoveStart(GridLayout grid, GameObject brushTarget, BoundsInt position) 
```

`MoveStart` 使用 `GridLayout` 网格根据给定范围来标记从目标游戏对象 `brushTarget` 进行移动的起点。在网格上激活画笔并在 Palette 窗口中选择__移动工具 (Move Tool)__ 并首次触发 `Move` (`MouseDown`) 时会触发此 API。重写此 API 可以在从目标开始移动时实现所需的操作。通常，这是从给定起始位置的目标选取数据。

```
public virtual void MoveEnd(GridLayout grid, GameObject brushTarget, BoundsInt position) 
```

`MoveEnd` 使用 `GridLayout` 网格根据给定范围来标记从目标游戏对象 `brushTarget` 进行移动的终点。在网格上激活画笔并在 Palette 窗口中选择__移动工具 (Move Tool)__ 并完成 `Move` (`MouseUp`) 时会触发此 API。重写此 API 可以在从目标结束移动时实现所需的操作。通常，这是向给定最终位置的目标绘制数据。

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
