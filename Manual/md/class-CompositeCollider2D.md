# 2D 复合碰撞体 (Composite Collider 2D)

2D 复合碰撞体[组件](UsingComponents.html)是用于 2D 物理的[碰撞体](CollidersOverview.html)。与大多数碰撞体不同，此碰撞体没有定义固有的形状。相反，此碰撞体将合并所设置的 [2D 盒型碰撞体 (Box Collider 2D)](class-BoxCollider2D.html) 或 [2D 多边形碰撞体 (Polygon Collider 2D)](class-PolygonCollider2D.html) 的形状。2D 复合碰撞体使用所有此类碰撞体的顶点（几何体），并将这些顶点合并为由 2D 复合碰撞体本身控制的新几何体。

2D 盒型碰撞体和 2D 多边形碰撞体组件具有 __Used By Composite__ 复选框。勾选此复选框即可将这些碰撞体附加到 2D 复合碰撞体。这些碰撞体还与 2D 复合碰撞体附加到同一 [2D 刚体](class-Rigidbody2D.html)。启用 __Used by Composite__ 时，其他属性会从该组件中消失，因为这些属性现在由附加的 2D 复合碰撞体控制。

请参阅关于 [2D 复合碰撞体](../ScriptReference/CompositeCollider2D.html)的 API 文档以了解有关使用 2D 复合碰撞体进行编程的更多信息。

![](../uploads/Main/class-CompositeCollider2D.png) 

| **属性**| **功能** |
|:---|:---| 
| __Density__ | 通过更改密度可更改[游戏对象](GameObjects.html)关联的 2D 刚体的质量计算。如果将该值设置为 0，则其关联的 2D 刚体将在所有质量计算（包括质心计算）中忽略 2D 碰撞体。请注意，只有在关联的 2D 刚体组件中启用 __Use Auto Mass__ 时，此选项才可用。 |
| __Material__ | 一种 [2D 物理材质](class-PhysicsMaterial2D.html)，可用于确定碰撞的属性（例如摩擦和弹性）。 |
| __Is Trigger__ | 如果希望 2D 复合碰撞体作为触发器运行，请选中此框（请参阅关于[碰撞体](CollidersOverview.html)的概述文档以了解有关触发器的更多信息）。 |
| __Used by Effector__ | 如果希望 2D 复合碰撞体由附加的 [2D 效应器](Effectors2D.html)组件使用，请选中此框。 |
| __Offset__| 设置 2D 碰撞体几何形状的局部偏移。 |
| __Geometry Type__ | 合并碰撞体时，所选碰撞体的顶点将组合为两种不同几何体类型之一。使用此下拉选单，可将几何体类型设置为 __Outlines__ 或 __Polygons__。  |
|&nbsp;&nbsp;&nbsp;&nbsp;Outlines| 生成具有空心轮廓的 2D 碰撞体，与 [2D 边界碰撞体 (Edge Collider 2D)](class-EdgeCollider2D.html) 生成的结果一样。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Polygons| 生成具有实心多边形的 2D 碰撞体，与 [2D 多边形碰撞体 (Polygon Collider 2D)](class-PolygonCollider2D.html) 生成的结果一样。 |
| __Generation Type__ | 该方法用于控制在更改 2D 复合碰撞体时或者更改其任何成员碰撞体时何时生成几何体。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Synchronous| 对 2D 复合碰撞体或其使用的任何碰撞体进行更改时，Unity 立即生成新几何体。 |
|&nbsp;&nbsp;&nbsp;&nbsp;Manual| 仅在手动请求时才生成新几何体。要请求生成几何体，请调用 [CompositeCollider2D.GenerateGeometry](../ScriptReference/CompositeCollider2D.GenerateGeometry.html) 脚本 API，或者单击选择项下方显示的 __Regenerate Geometry__ 按钮。 |
| __Vertex Distance__| 设置从复合碰撞体收集的任何顶点允许的最小间距值。比此限值更近的任何顶点都将被删除。此设置可用于控制顶点合成的有效分辨率。 |
| __Edge Radius__| 控制边缘周围的半径，使顶点为圆形。这会产生一个具有圆凸角的更大 2D 碰撞体。此设置的默认值是 0（无半径）。仅当 __Geometry Type__ 设置为 __Outlines__ 时，此设置才有效。 |
