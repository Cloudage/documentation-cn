# TileBase

所有要添加到瓦片地图的瓦片必须继承自 `TileBase`。`TileBase` 为瓦片地图提供了一组固定 API 来传达其渲染属性。对于 API 的大多数情况，瓦片的位置和瓦片所在瓦片地图的实例作为 API 的参数传入。由此可确定用于设置瓦片信息的所有必需属性。

```
public void RefreshTile(Vector3Int location, ITilemap tilemap) 
```

`RefreshTile` 确定将此瓦片添加到瓦片地图时所更新的附近瓦片。默认情况下，`TileBase` 调用 `tilemap.RefreshTile(location)` 来刷新当前位置的瓦片。对此进行重写可以确定由于放置新瓦片而需要刷新的瓦片。

**示例：**在一条直路的旁边放着一个 `RoadTile`。直路不再有效。实际需要一个 T 形截面。Unity 不会自动知道需要刷新什么，因此 `RoadTile` 需要触发自身刷新，但也需要触发邻近道路上的刷新。

```
public bool GetTileData(Vector3Int location, ITilemap tilemap, ref TileData tileData) 
```

`GetTileData` 确定瓦片在瓦片地图上的外观。有关更多详细信息，请参阅后面的 `TileData`。

```
public bool GetTileAnimationData(Vector3Int location, ITilemap tilemap, ref TileAnimationData tileAnimationData) 
```

`GetTileAnimationData` 确定瓦片是否已动画化。如果瓦片有动画，则返回 true，否则返回 false。

```
public bool StartUp(Vector3Int location, ITilemap tilemap, GameObject go) 
```

当瓦片地图第一次更新时，将为每个瓦片调用 `StartUp`。如有必要，可为瓦片地图上的瓦片运行任何启动逻辑。参数 *go* 是调用 `GetTileData` 时作为游戏对象而传入的对象的实例化版本。也可根据需要更新 *go*。

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
