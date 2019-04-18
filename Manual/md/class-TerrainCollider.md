#地形碰撞体

__地形碰撞体 (Terrain Collider)__ 实现了一个碰撞表面，其形状与其所附加到的 [Terrain](script-Terrain.html) 对象相同。

![](../uploads/Main/Inspector-TerrainCollider.png) 


##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Material__ |引用[物理材质](class-PhysicMaterial.html)，可确定该碰撞体与其他对象的交互方式。 |
|__Terrain Data__ |地形数据。 |
|__Enable Tree Colliders__ |选中此属性时，将启用树碰撞体。 |

##详细信息

应注意，Unity 5.0 之前的版本中，地形碰撞体具有 __Smooth Sphere Collisions__ 属性，用于改善地形和球体之间的相互作用。此属性现已废弃，因为平滑交互是物理引擎的标准行为，将其关闭没有特别的优势。
