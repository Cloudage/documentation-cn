#网格过滤器 (Mesh Filter)

__网格过滤器 (Mesh Filter)__ 从资源中获取网格并将其传递给[网格渲染器 (Mesh Renderer)](class-MeshRenderer.html) 以便在屏幕上渲染。

![](../uploads/Main/Inspector-MeshFilter.png) 



##属性

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Mesh__ |引用将要渲染的[网格](class-Mesh.html)。__网格__ 位于 Assets 目录中。 |


##详细信息

导入网格资源时，如果网格带蒙皮，则 Unity 会自动创建[带蒙皮的网格渲染器 (Skinned Mesh Renderer)](class-SkinnedMeshRenderer.html)，而如果网格不带蒙皮，则创建网格过滤器 (Mesh Filter) 及网格渲染器 (Mesh Renderer)。

要在场景中查看网格，请向游戏对象添加[网格渲染器](class-MeshRenderer.html)。此渲染器应该会自动添加，但如果从对象上删除了此渲染器，必须手动重新进行添加。如果网格渲染器不存在，网格仍将存在于场景（和计算机内存）中，但不会绘制网格。

