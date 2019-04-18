# 运动

当导入的动画剪辑包含根运动时，Unity 会使用该运动来驱动正在播放动画的游戏对象的移动和旋转。但是，有时可能需要手动选择动画文件层级视图中的其他特定节点作为根运动节点。

借助动画导入设置中的 Motion 字段，可以使用层级弹出菜单来选择位于导入动画层级视图中的任何节点 (Transform) 并将其用作根运动源。该对象的动画位置和旋转可驱动播放动画的游戏对象的动画位置和旋转。

要选择动画的根运动节点，请展开 Motion 部分以显示 Root Motion Node 菜单。打开该菜单时会显示导入文件层级视图根目录中的所有对象，包括 *None* 和 *Root Transform*。这些对象可能包括角色的网格对象及其根骨骼名称，可能还包括具有子对象的每个项目的子菜单。每个子菜单还包含子对象本身，如果这些对象也具有子对象，还会包含进一步的子菜单。

![遍历对象层级视图以选择根运动节点](../uploads/Main/AnimationInspectorRootNodeSelectionMenu.png)

选择根运动节点后，对象的动画将驱动其运动。


---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
