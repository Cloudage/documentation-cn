#AssetBundle 依赖项

如果一个或多个 `UnityEngine.Objects` 包含对位于另一个捆绑包中的 `UnityEngine.Object` 的引用，则 AssetBundle 可以变为依赖于其他 AssetBundle。如果 `UnityEngine.Object` 包含对任何 AssetBundle 中未包含的 `UnityEngine.Object` 的引用，则不会发生依赖关系。在这种情况下，在构建 AssetBundle 时，捆绑包所依赖的对象的副本将复制到捆绑包中。如果多个捆绑包中的多个对象包含对未分配给捆绑包的同一对象的引用，则每个对该对象具有依赖关系的捆绑包将创建其自己的对象副本并将其打包到构建的 AssetBundle 中。

如果 AssetBundle 中包含依赖项，则在加载尝试实例化的对象之前，务必加载包含这些依赖项的捆绑包。Unity 不会尝试自动加载依赖项。

参考以下示例，__Bundle 1__ 中的材质引用了 __Bundle 2__ 中的纹理：

在此示例中，在从 __Bundle 1__ 加载材质之前，需要将 __Bundle 2__ 加载到内存中。加载 __Bundle 1__ 和 __Bundle 2__ 的顺序无关紧要，重要的是在从 __Bundle 1__ 加载材质之前应加载 __Bundle 2__。在下一部分，我们将讨论如何使用我们在上一部分介绍的 `AssetBundleManifest` 对象在运行时确定并加载依赖项。

----

* <span class="page-edit">2017-05-15  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

