# 其他有用的类

## TileFlags

`
None = 0
`

不为瓦片设置任何标志。这是大多数瓦片的默认设置。

`
LockColor = 1 << 0
`

如果瓦片脚本控制瓦片的颜色，请设置此标志。如果进行此设置，则瓦片将控制瓦片放置在瓦片地图上时的颜色。无法通过绘制或使用脚本来更改瓦片的颜色。如果未进行此设置，则可以通过绘制或使用脚本来更改瓦片的颜色。

`
LockTransform = 1 << 1
`

如果瓦片脚本控制瓦片的变换，请设置此标志。如果进行此设置，则瓦片将控制瓦片放置在瓦片地图上时的变换。无法通过绘制或使用脚本来旋转或更改瓦片的变换。如果未进行此设置，则可以通过绘制或使用脚本来更改瓦片的变换。

`
LockAll = LockColor | LockTransform
`

这是 TileBase 使用的所有锁定标志的组合。

`
InstantiateSpawnGameObjectRuntimeOnly = 1 << 2
`

如果瓦片脚本仅应当在项目运行时且不在 Editor 模式下才生成其游戏对象，请设置此标志。



## Tile.ColliderType

`
None = 0
`

此瓦片不生成任何碰撞体形状。

`
Sprite = 1
`

此瓦片生成的碰撞体形状是由瓦片返回的精灵所设置的物理形状。如果在精灵中未设置任何物理形状，则会尝试根据精灵的轮廓生成一个形状。

注意：如果需要在运行时生成瓦片的碰撞体形状，请为精灵设置一个物理形状或将精灵的纹理设置为可读，以便 Unity 根据轮廓生成形状。

`
Grid = 2
`

此瓦片生成的碰撞体形状是由网格布局定义的单元格的形状。

## ITilemap

`ITilemap` 是基类；在此基类中，当瓦片地图尝试从瓦片检索数据时，`Tile` 可从瓦片地图检索数据。

```
Vector3Int origin { get; }
```

返回瓦片地图在单元格空间中的原点。

```
Vector3Int size { get; }
```

返回瓦片地图在单元格空间中的大小。

```
Bounds localBounds { get; }
```

返回瓦片地图在局部空间中的边界。

```
BoundsInt cellBounds { get; }
```

返回瓦片地图在单元格空间中的边界。

```
Sprite GetSprite(Vector3Int location);
```

返回瓦片地图中给定位置的瓦片使用的精灵。

```
Color GetColor(Vector3Int location);
```

返回瓦片地图中给定位置的瓦片使用的颜色。

```
Matrix4x4 GetTransformMatrix(Vector3Int location);
```

返回瓦片地图中给定位置的瓦片使用的变换矩阵。

```
TileFlags GetTileFlags(Vector3Int location);
```

返回瓦片地图中给定位置的瓦片使用的瓦片标志。

```
TileBase GetTile(Vector3Int location);
```

返回瓦片地图中给定位置的瓦片。如果该位置没有瓦片，则返回 null。

```
T GetTile<T>(Vector3Int location) where T : TileBase;
```

返回瓦片地图中给定位置的 T 类型瓦片。如果该位置没有与该类型匹配的瓦片，则返回 null。

```
void RefreshTile(Vector3Int location);
```

请求刷新瓦片地图中给定位置的瓦片。

```
T GetComponent<T>();
```

返回附加到瓦片地图游戏对象的组件 T。

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
