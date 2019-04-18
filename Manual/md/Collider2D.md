# 2D 碰撞体

__2D 碰撞体__组件可定义用于物理碰撞的 2D 游戏对象的形状。碰撞体是不可见的，其形状不需要与游戏对象的网格完全相同；事实上，粗略近似方法通常更有效，在游戏运行过程中难以察觉。

2D 游戏对象的所有碰撞体的名称都以“2D”结尾。名称中没有“2D”的碰撞体将用于 3D 游戏对象。请注意，不能混用 3D 游戏对象和 2D 碰撞体，也不能混用 2D 游戏对象和 3D 碰撞体。

可用于 2D 刚体的 2D 碰撞体类型为：

* 用于圆形碰撞区域的 [2D 圆形碰撞体](class-CircleCollider2D.html)。
* 用于正方形和矩形碰撞区域的 [2D 盒型碰撞体](class-BoxCollider2D.html)。
* 用于自由形状碰撞区域的 [2D 多边形碰撞体](class-PolygonCollider2D.html)。
* 用于自由形状碰撞区域和非全封闭区域（例如圆凸角）的 [2D 边界碰撞体](class-EdgeCollider2D.html)。
* 用于圆形或菱形碰撞区域的 [2D 胶囊碰撞体](class-CapsuleCollider2D.html)。
* 用于合并 2D 盒型碰撞体与 2D 多边形碰撞体的 [2D 复合碰撞体](class-CompositeCollider2D.html)。
 

## Use Auto Mass

在 2D 刚体组件上，勾选 __Use Auto Mass__ 复选框可将 2D 刚体的质量自动设置为与 2D 碰撞体质量相同的值。与 [2D 浮力效应器 (Buoyancy Effector 2D)](class-BuoyancyEffector2D.html) 结合时，此设置尤其有用。
