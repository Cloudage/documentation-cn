# 瓦片贴图和 2D 物理

* 可将 Tilemap Collider 2D 组件添加到瓦片地图的游戏对象来根据瓦片地图的瓦片生成碰撞体。

* Tilemap Collider 2D 组件的功能类似于普通的 Collider 2D 组件。可添加 Effector 2D 来修改 Tilemap Collider 2D 的行为。还可以使用 [Composite Collider 2D](class-CompositeCollider2D.html) 来合成 Tilemap Collider 2D。

* 如果添加或删除 __Collider Type__ 设置为 __Sprite__ 或 __Grid__ 的瓦片，则会在 Tilemap Collider 2D 的下一次 `LateUpdate` 时添加或删除瓦片在 Tilemap Collider 2D 组件中的碰撞体形状。当 Tilemap Collider 2D 由 Composite Collider 2D 合成时，也会发生这种情况。

---

* <span class="page-edit">2017-09-06 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
