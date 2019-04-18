# 从 3D 建模软件导入模型

有两种方法可以将模型导入 Unity：

* 将模型文件从文件浏览器直接拖到 Unity Project 窗口中。
* 将模型文件复制到项目的 Assets 文件夹中。

在 __Project__ 视图中选择文件，然后导航到 Inspector 窗口中的 __Model__ 选项卡以配置导入选项。请参阅 [Model Import Settings 窗口] 参考文档以了解详细信息。

***注意：***必须将纹理存储在名为 __Textures__ 的文件夹中，而该文件夹位于 Unity 项目的 __Assets__ 文件夹（在导出的网格旁边）内。这种做法使 Unity Editor 能够找到纹理并将它们连接到生成的材质。有关更多信息，请参阅[导入纹理](class-TextureImporter.html)文档。


## 另请参阅

* [创建优化的角色模型](ModelingOptimizedCharacters.html)
* [网格导入设置](class-FBXImporter.html)
* [网格组件](class-Mesh.html)
* [修复导入模型的旋转问题](HOWTO-FixZAxisIsUp.html)

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
