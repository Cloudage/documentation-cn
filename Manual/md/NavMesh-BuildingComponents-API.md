# 导航网格构建组件 API

[导航网格](nav-NavigationSystem.html)构建[组件](UsingComponents.html)为您提供在运行时以及在 Unity Editor 中构建（也称为[烘焙](nav-BuildingNavMesh.html)）和使用导航网格的额外控制力。

导航网格修改器不在 Unity 标准安装中；有关如何访问这些组件的信息，请参阅[高级导航网格构建组件](NavMesh-BuildingComponents.html)的文档。

### 导航网格表面 (NavMeshSurface)

#### 属性

* `agentTypeID` – 需要构建导航网格的代理类型的 ID。
* `collectObjects` – 定义如何从场景收集输入几何体，为 `UnityEngine.AI.CollectObjects` 之一：
    * `All` – 使用场景中的所有对象。
    * `Volume` – 使用场景中与包围体接触的所有游戏对象（请参阅 `size` 和 `center`）
    * `Children` – 使用导航网格表面 (NavMesh Surface) 附加到的游戏对象的所有子对象。
* `size` – 构建体积的尺寸。该大小不受缩放影响。
* `center` – 构建体积的中心（相对于变换中心）。
* `layerMask` – 位掩码，用于定义必须将哪些层上的游戏对象包含在烘焙中。
* `useGeometry` – 定义用于烘焙的几何体，为 `UnityEngine.NavMeshCollectGeometry` 之一：
    * `RenderMeshes` – 使用渲染网格和地形中的几何体
    * `PhysicsColliders` – 使用碰撞体和地形中的几何体。
* `defaultArea` – 所有输入几何体的默认区域类型（除非另有说明）。
* `ignoreNavMeshAgent` – 如果具有导航网格代理 (Nav Mesh Agent) 组件的游戏对象应作为输入被忽略，则为 True。
* `ignoreNavMeshObstacle` – 如果具有导航网格障碍物 (Nav Mesh Obstacle) 组件的游戏对象应作为输入被忽略，则为 True。
* `overrideTileSize` – 如果设置了区块大小，则为 True。
* `tileSize` – 以体素为单位的区块大小（组件描述包含有关如何选择区块大小的信息）。
* `overrideVoxelSize` – 如果设置了体素大小，则为 True。
* `voxelSize` – 以世界单位表示的体素大小（组件描述包含有关如何选择区块大小的信息）。
* `buildHeightMesh` – 未实现。
* `bakedNavMeshData` – 对表面使用的 NavMeshData 的引用，如果未设置，则为 null。
* `activeSurfaces` – 所有激活状态的导航网格表面的列表。

__注意：__上述值会影响烘焙的结果，因此必须调用 `Bake()` 来包含它们。

#### 公共函数

* `void Bake ()`

根据导航网格表面上设置的参数烘焙新的 NavMeshData。可通过 `bakedNavMeshData` 访问该数据。

### 导航网格修改器 (NavMesh Modifier)

#### 属性

* `overrideArea` – 如果修改器覆盖区域类型，则为 True。
* `area` – 要应用的新区域类型。
* `ignoreFromBuild` – 如果包含修改器的游戏对象及其子项不应当用于导航网格烘焙，则为 True。
* `activeModifiers` – 所有激活状态的导航网格修改器的列表。

#### 公共函数

* `bool AffectsAgentType(int agentTypeID)`

如果修改器应用于指定的代理类型，则返回 true，否则返回 false。

### 导航网格修改器体积 (NavMesh Modifier Volume)

#### 属性

* `size` – 包围体的大小（采用局部空间单位）。变换会影响该大小。
* `center` – 包围体的中心（采用局部空间单位）。变换会影响该中心。
* `area ` – 要应用于包围体内的导航网格区域的区域类型。

#### 公共函数

* `bool AffectsAgentType(int agentTypeID)`

如果修改器应用于指定的代理类型，则返回 true。

### 导航网格链接 (NavMesh Link)

#### 属性

* `agentTypeID` – 可使用该链接的代理类型。
* `startPoint` – 链接的起点（采用局部空间单位）。变换会影响该位置。
* `endPoint` – 链接的终点（采用局部空间单位）。变换会影响该位置。
* `width` – 链接的宽度（采用世界长度单位）。
* `bidirectional` – 如果为 true，则可以双向遍历链接。如果为 false，只能按照从起点到终点的方向遍历链接。
* `autoUpdate` – 如果为 true，则链接将更新端点以跟随每帧的游戏对象变换。
* `area ` – 链接的区域类型（用于计算寻路成本）。

#### 公共函数

* `void UpdateLink()`

更新链接以便与关联的变换匹配。这对于更新链接很有用（例如在更改变换位置之后），但如果启用了 `autoUpdate` 属性，则不需要。但是，如果您很少更改链接变换，则调用 `UpdateLink` 可能对性能产生的影响要小得多。

<br/><br/> 
---

* <span class="page-edit"> 2017-05-26  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.6 中的新功能</span>

