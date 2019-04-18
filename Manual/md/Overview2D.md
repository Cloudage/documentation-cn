#2D 游戏

虽然 Unity 以 3D 功能而闻名，但也可用于创建 2D 游戏。熟悉的 Editor 功能仍然可用，但还添加了有助于简化 2D 开发的功能。

![在 2D 模式下显示的场景](../uploads/Main/Overview2D.jpg)

最明显的功能是 Scene 视图工具栏中的 _2D_ 视图模式按钮。启用 2D 模式时将会设置正交（即无透视）视图：摄像机沿 Z 轴观察，而 Y 轴向上增加。因此可以轻松可视化场景并放置 2D 对象。

有关 2D 组件的完整列表、如何在 2D 和 3D 模式之间切换以及不同的 2D 和 3D 模式设置，请参阅 [2D 还是 3D 项目](2Dor3D.html)。



##2D 图形

2D 图形对象称为__精灵__。精灵本质上只是标准纹理，但可通过一些特殊技巧在开发过程中组合和管理精灵纹理以提高效率和方便性。Unity 提供内置的 [Sprite Editor](SpriteEditor.html)，允许从更大图像提取精灵图形。因此可以在图像编辑器中编辑单个纹理内的多个组件图像。例如，可以使用此工具将角色的手臂、腿和身体保持为一个图像中的单独元素。

应使用 [Sprite Renderer](class-SpriteRenderer.html) 组件而不是用于 3D 对象的 [Mesh Renderer](class-MeshRenderer.html) 来渲染精灵。可通过 Components 菜单 (__Component > Rendering > Sprite Renderer__) 将精灵渲染器 (Sprite Renderer) 添加到游戏对象，也可直接创建已附加精灵渲染器的游戏对象（菜单：__GameObject &gt; 2D Object &gt; Sprite__）。

此外，可以使用 [Sprite Creator](SpriteCreator.html) 工具来创建 2D 占位图像。


##2D 物理

Unity 有一个独立物理引擎来处理 2D 物理，以便利用仅适用于 2D 的优化。2D 物理组件对应于标准 3D 物理组件（例如[刚体 (Rigidbody)](class-Rigidbody.html)、[盒型碰撞体 (Box Collider)](class-BoxCollider.html) 和[铰链关节 (Hinge Joint)](class-HingeJoint.html)，但名称中附加了“2D”字样。因此，精灵可以配备 [2D 刚体 (Rigidbody 2D)](class-Rigidbody2D.html)、[2D 盒型碰撞体 (Box Collider 2D)](class-BoxCollider2D.html) 和 [2D 铰链关节 (Hinge Joint 2D)](class-HingeJoint2D.html)。大多数 2D 物理组件都是 3D 对等组件的简单“平坦”版本（例如，_2D 盒型碰撞体_是正方形，而_盒型碰撞体_是立方体），但是也有一些例外。

有关 2D 物理组件的完整列表，请参阅 [2D 还是 3D 项目](2Dor3D.html)。请参阅手册的[物理系统](PhysicsSection.html)部分以了解关于 2D 物理系统概念和组件的更多信息。要指定 2D 物理设置，请参阅 [Physics 2D Settings](class-Physics2DManager.html)。

---

* <span class="page-edit">2018-04-24 Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
