如何创建网格粒子发射器？（旧版粒子系统）
===============================================================


需要高度控制发射粒子的位置时，通常使用__网格粒子发射器__。

例如，当您想要创建一把火焰之剑时：


1.将网格拖入场景中。
1.通过右键单击__网格渲染器 (Mesh Renderer)__ 的 __Inspector__ 标题栏并选择 __Remove Component__，将__网格渲染器__删除。
1.从 __Component &gt; Effects &gt; Legacy Particles__ 菜单中选择 __Mesh Particle Emitter__。
1.从 __Component &gt; Effects &gt; Legacy Particles__ 菜单中选择 __Particle Animator__。
1.从 __Component &gt; Effects &gt; Legacy Particles__ 菜单中选择 __Particle Renderer__。

现在应该会看到从网格发射粒子。

尝试在[网格粒子发射器](class-MeshParticleEmitter.html)中设置不同值。

尤其需要在网格粒子发射器 (Mesh Particle Emitter) 的 Inspector 中启用 __Interpolate Triangles__，并将 __Min Normal Velocity__ 和 __Max Normal Velocity__ 设置为 1。

要自定义发射的粒子的外观，请执行以下操作：


1.从菜单栏中选择 __Assets &gt; Create &gt; Material__。
1.在材质检视面板 (Inspector) 中，从 Shader 下拉选单选择 __Particles &gt; Additive__。
1.将纹理从 __Project 视图__拖放到材质检视面板中的纹理字段上。
1.将材质从 Project 视图拖动到 __Scene 视图__中的粒子系统上。

现在应该会看到从网格发射纹理化粒子。

另请参阅
--------


* [网格粒子发射器组件参考页面](class-MeshParticleEmitter.html)
