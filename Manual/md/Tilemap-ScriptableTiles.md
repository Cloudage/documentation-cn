# 可编程瓦片

##创建可编程瓦片

创建一个继承自 `TileBase`（或 `TileBase` 的任何有用子类，如 `Tile`）的新类。重写新的 `Tile` 类所需的所有方法。以下是将要重写的常用方法：

* `RefreshTile` 确定将此瓦片添加到瓦片地图时所更新的附近瓦片。
* `GetTileData` 确定瓦片在瓦片地图上的外观。

使用 `ScriptableObject.CreateInstance<YOUR_TILE_CLASS>()` 创建该新类的实例。可通过调用 `AssetDatabase.CreateAsset()` 将此新实例转换为 Editor 中的资源以便重复使用。

还可以为瓦片创建自定义编辑器。这与脚本化对象的自定义编辑器的工作方式相同。

请记住保存项目以便确保对新瓦片资源进行保存！

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

