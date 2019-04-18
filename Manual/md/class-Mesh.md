# 网格

__网格__构成了 3D 世界的很大一部分。除了一些 [Asset Store](http://unity3d.com/unity/asset-store/) 插件外，Unity 不提供建模工具。然而，Unity 与大多数 3D 建模软件包之间有很好的交互性。Unity 支持三角形或四边形的多边形网格。Nurbs、Nurms、Subdiv 曲面必须转换为多边形。


![](../uploads/Main/Tris.png) 



## 纹理

Unity 将尝试通过遵循特定的搜索计划在导入时自动查找网格使用的纹理。首先，导入器将在与网格相同的文件夹中或任何父文件夹中查找名为 Textures 的子文件夹。如果此过程失败，将对项目中的所有纹理执行穷尽搜索。虽然速度只是稍慢，但穷尽搜索的主要缺点是项目中可能有两个或多个同名的纹理。在这种情况下，不能保证找到合适的纹理。


![纹理放置在资源级别或更高级别的 __Textures__ 文件夹中](../uploads/Main/Mesh-TextureImportHierarchy.png)


[Import Settings 窗口的 Material 选项卡](FBXImporter-Materials.html)


## 材质生成和分配

对于每种导入的材质，Unity 将应用以下规则：

如果禁用材质生成（即取消选中 __Import Materials__），则会分配 Default-Diffuse 材质。如果启用该选项，则执行以下操作：

* Unity 将根据 __Material Naming__ 设置，为 Unity 材质选择一个名称
* Unity 将尝试查找具有该名称的现有材质。材质搜索范围由 __Material Search__ 设置进行定义。
* 如果 Unity 成功找到现有材质，则会将该材质用于导入的场景，否则将生成新材质


## 碰撞体

Unity 使用两种主要类型的碰撞体：__网格碰撞体__和__原始碰撞体__。网格碰撞体是使用导入网格数据的组件，可用于环境碰撞。在 Import Settings 中启用 __Generate Colliders__ 后，将网格添加到场景时会自动添加网格碰撞体。就物理系统而言，网格碰撞体被认为是实体的。

如果对象正在移动（例如汽车），则无法使用网格碰撞体。必须改用原始碰撞体。此情况下应禁用 __Generate Colliders__ 设置。


## 动画

可从模型文件[导入动画](ImportingModelFiles.html)。请先遵循[从 3D 建模软件导出 FBX 文件](HOWTO-exportFBX.html)的指南，再将文件导入 Unity。


## 法线贴图和角色

如果一个角色具有从模型的复杂多边形版本生成的法线贴图，则应导入__平滑角__为 180 度的游戏质量版本。这样将防止由于切线分裂而导致光照中出现看起来奇怪的接缝。如果使用这些设置后仍然存在接缝，请启用 __Split tangents across UV seams__。

如果是将灰度图像转换为法线贴图，则无需担心这一点。


## 混合形状 (Blendshape)

Unity 支持 BlendShape（也称为变形目标或顶点级动画）。Unity 可从 **.FBX**（BlendShape 和控制动画）和 **.dae**（仅 BlendShape）导出的 3D 文件导入 BlendShape。Unity BlendShape 支持顶点、法线和切线上的顶点级动画。网格可能同时受到皮肤和 BlendShape 的影响。使用 BlendShape 导入的所有网格都将使用 SkinnedMeshRenderer（无论其是否有皮肤）。BlendShape 动画作为常规动画的一部分导入，仅对 SkinnedMeshRenderer 上的 BlendShape 权重进行动画化。

有两种方法可以导入带法线的 BlendShape：

1.将 __Normals__ 导入模式设置为 __Calculate__，这样就会使用相同的逻辑来计算网格和 BlendShape 上的法线。
1.将平滑组信息导出到源文件。这样，Unity 就会通过网格和 BlendShape 的平滑组计算法线。

如果需要 BlendShape 上的切线，则应将 __Tangents__ 导入模式设置为 __Calculate__。


## 提示

* 尽可能将网格合并在一起。让它们共享材质和纹理。这种做法可大幅提升性能。
* 如果需要在 Unity 中进一步设置对象（添加物理设置、脚本或其他酷炫功能），为了给自己减少麻烦，请在 3D 应用程序中妥善命名对象。处理大量 _pCube17_ 或 _Box42_ 之类的对象可一点都不好玩。
* 使网格在 3D 应用程序中的世界原点居中。这样将使网格更容易在 Unity 中定位。
* 如果网格没有顶点颜色，Unity 将在第一次渲染网格时自动将全白色顶点颜色数组添加到网格。

### Unity Editor 显示太多顶点或三角形（与 3D 应用程序之类的相比）
这是正常的。您正在查看的是实际发送到 GPU 进行渲染的顶点/三角形数量。除了材质要求发送两次这些顶点/三角形的情况之外，其他诸如硬法线和非连续 UV 的元素与建模应用程序显示的情况相比会显著增加顶点/三角形数量。三角形在 3D 和 UV 空间中都需要处于连续状态以形成条带，因此当有 UV 接缝时，必须使三角形退化以形成条带，而这会增加计数。

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
