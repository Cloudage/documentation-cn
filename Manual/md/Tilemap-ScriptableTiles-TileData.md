# TileData

```
public Sprite sprite 
```

为瓦片渲染的精灵。

```
public Color color 
```

为瓦片使用的精灵的着色颜色。

```
public Matrix4x4 transform 
```

用于确定瓦片最终位置的变换矩阵。对此进行修改可向瓦片添加旋转或缩放。

```
public GameObject gameobject 
```

将瓦片添加到瓦片地图时实例化的游戏对象。

```
public TileFlags flags 
```

用于控制瓦片行为的标志。有关更多详细信息，请参阅前面的 TileFlags。

```
public Tile.ColliderType colliderType 
```

控制由瓦片为附加的 Tilemap Collider 2D 组件生成的碰撞体形状。有关更多详细信息，请参阅有关 `Tile.ColliderType` 的文档。

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
