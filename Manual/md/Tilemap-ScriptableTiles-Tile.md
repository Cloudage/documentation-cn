# Tile

`Tile` 类是一个允许在瓦片地图上渲染精灵的简单类。Tile 继承自 `TileBase`。下面描述了为具有 Tile 的行为而要重写的方法。

```
public Sprite sprite;
public Color color = Color.white;
public Matrix4x4 transform = Matrix4x4.identity;
public GameObject gameobject = null;
public TileFlags flags = TileFlags.LockColor; 
```

```
public ColliderType colliderType = ColliderType.Sprite; 
```

这些是 Tile 的默认属性。如果通过将精灵拖放到 Tilemap Palette 上来创建瓦片，则瓦片会将 __Sprite__ 属性设置为已放入的精灵。可调整瓦片实例的属性来获取所需的瓦片。

```
public void RefreshTile(Vector3Int location, ITilemap tilemap) 
```

不会由 `TileBase` 重写。默认情况下，仅刷新该位置的瓦片。

```
public override void GetTileData(Vector3Int location, ITilemap tilemap, ref TileData tileData)
{
	tileData.sprite = this.sprite;
	tileData.color = this.color;
	tileData.transform = this.transform;
	tileData.gameobject = this.gameobject;
	tileData.flags = this.flags;

tileData.colliderType = this.colliderType;
} 
```

通过将 Tile 实例的属性复制到 `tileData` 来填充瓦片地图渲染瓦片时所需的信息。

```
public bool GetTileAnimationData(Vector3Int location, ITilemap tilemap, ref TileAnimationData tileAnimationData) 
```

不会由 TileBase 重写。默认情况下，Tile 类不运行任何瓦片动画并会返回 false。

```
public bool StartUp(Vector3Int location, ITilemap tilemap, GameObject go) 
```

不会由 `TileBase` 重写。默认情况下，`Tile` 类没有任何特殊的启动功能。如果设置了 `tileData.gameobject`，则瓦片地图仍会在启动时将其实例化并将其放置在瓦片的位置。

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
