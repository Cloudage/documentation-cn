# 导航网格构建组件

[导航网格](nav-NavigationSystem.html)构建[组件](UsingComponents.html)为您提供在运行时以及在 Unity Editor 中构建（也称为[烘焙](nav-BuildingNavMesh.html)）和使用导航网格的额外控制力。

从 [Unity 商店](https://store.unity.com/)下载的标准 Unity Editor 安装程序中未提供下列高级导航网格构建组件。请从 [Unity Technologies GitHub](https://github.com/Unity-Technologies/NavMeshComponents) 下载这些组件并单独安装。

导航网格有四个配套的高级组件：

* [导航网格表面 (NavMesh Surface)](class-NavMeshSurface.html) - 用于为一种类型的代理构建和启用导航网格表面。

* [导航网格修改器 (NavMesh Modifier)](class-NavMeshModifier.html) - 用于根据变换层级视图来影响导航网格区域类型的导航网格生成。

* [导航网格修改器体积 (NavMeshModifierVolume)](class-NavMesh-ModifierVolume.html) - 用于根据体积来影响导航网格区域类型的导航网格生成。

* [导航网格链接 (NavMeshLink)](class-NavMeshLink.html) - 用于为一种类型的代理连接相同或不同的导航网格表面。


另请参阅有关 [Mesh-BuildingComponents-API](NavMesh-BuildingComponents-API.html) 的文档。

有关代理类型的更多信息，请参阅[创建导航网格代理](nav-CreateNavMeshAgent.html)的相关文档。

有关导航网格区域类型的更多详细信息，请参阅[导航网格区域](nav-AreasAndCosts.html)的相关文档。



## 创建高级导航网格构建组件

要安装高级导航网格构建组件，请执行以下操作：

1.[下载并安装](https://store.unity.com/) Unity 5.6 或更高版本。

2.在 Unity Technologies GitHub 上的[导航网格组件 (NavMesh Components) 页面](https://github.com/Unity-Technologies/NavMeshComponents)中，单击绿色的 __Clone or download__ 按钮以克隆或下载代码仓库。

3.使用 Unity 打开导航网格组件项目 (NavMesh Components Project)，或者将 *A**ssets/NavMeshComponents* 文件夹的内容复制到现有项目。

可在 *Assets/Examples* 文件夹中查找其他示例。

__注意：__确保在安装高级导航网格构建组件之前备份项目。

<br/><br/> 

--------

* <span class="page-edit"> 2017-05-26  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.6 中的新功能</span>
