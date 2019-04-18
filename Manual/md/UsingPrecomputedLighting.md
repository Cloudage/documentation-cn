# 使用预计算光照

在 Unity 中，预计算光照在后台计算为自动过程或手动启动时进行计算。在这两种情况下，都可以在 Editor 中继续工作，同时让这些过程在后台运行。

预计算过程运行时，Editor 右下角会出现一个蓝色进度条。根据是否启用 Baked GI 或 Precomputed Realtime GI，需要完成不同的阶段。有关当前过程的信息会显示在进度条上。

![](../uploads/GlobalIllumination/BakingJobs.png) 

进度条显示 Unity 的预计算的当前状态。

在上面的示例中，我们可以看到目前处于 11 个任务中的第 5 个任务，即“聚类”(Clustering)，在该任务完成并在预计算转到第 6 个任务之前还剩余 6 个作业。下面列出了各个阶段：

__预计算实时全局光照 (Precomputed Realtime GI)__

1.创建几何体 (Create Geometry)
2.布局系统 (Layout Systems)
3.创建系统 (Create Systems)
4.创建图集 (Create Atlas)
5.聚类（Clustering）
6.可见性 (Visibility)
7.光线传输 (Light Transport)
8.四面体化探针 (Tetrahedralize Probes)
9.创建探针集 (Create ProbeSet)
								
__探针 (Probes)__

1.环境探针 (Ambient Probes)
2.烘焙/实时参考探针 (Baked/Realtime Ref. Probes)

__烘焙 GI (Baked GI)__

1.创建几何体 (Create Geometry)
2.镶嵌 (Atlassing)
3.创建烘焙系统 (Create Baked Systems)
4.烘焙资源 (Baked Resources)
5.烘焙 AO (Bake AO)
6.导出烘焙纹理 (Export Baked Texture)
7.烘焙可见性 (Bake Visibility)
8.烘焙直接光照 (Bake Direct)
9.环境光和发光 (Ambient and Emissive)
10.创建烘焙系统 (Create Bake Systems)
11.烘焙运行时 (Bake Runtime)
12.上采样可见性 (Upsampling Visibility)
13.烘焙间接光照 (Bake Indirect)
14.最终收集 (Final Gather)
15.烘焙探针集 (Bake ProbesSet)
16.合成 (Compositing)

##开始预计算

Unity 的预计算光照解决方案仅考虑静态几何体。要开始光照预计算过程，我们需要在场景中至少将一个游戏对象标记为“静态”。为此，可单独对某个游戏对象执行此操作，也可按住 Shift 键并从层级视图面板中选择多个游戏对象来执行此操作。

从 Inspector 面板中，可选中 Static 复选框 (**Inspector > Static**)。此操作将设置所有游戏对象的“静态选项”或“标志”（包括导航和批处理），这可能是不可取的。对于 Precomputed Realtime GI，仅需选中“Lightmap Static”。

为了实现更精细的控制，可从 Inspector 面板 Static 复选框右侧可访问的下拉列表中设置各个静态选项。此外，还可在 Lighting 窗口的 Object 区域中将对象设置为 Static。

如果将场景设置为 __Auto Generate__（菜单：__Window__ > __Lighting__ > __Settings__ > __Scene__ > __Auto Generate__），则 Unity 的光照预计算会自动开始，并在场景中的任何静态几何体发生变化时自动更新。如果未启用 __Auto Generate__，必须手动启动光照预计算。

##自动/手动预计算

如果在 Unity 的 Lighting 面板底部选中了 __Auto Generate__（菜单：__Window__ > __Lighting > Settings > Scene > Auto Generate__），只要场景中的静态几何体发生变化，此预计算就会自动作为后台进程开始运行。

但是，如果未启用 __Auto Generate__，必须手动启动光照预计算过程，方法是单击其旁边的“Build”按钮。这样就会以相同的方式开始预计算，并可让您控制此过程的开始时间。

__Auto Generate__ 在处理较小或较不复杂的场景时非常有用，因为它能在您移动或编辑场景中的静态游戏对象时快速生成准确的光照效果。但是，在处理大型或复杂场景时，您可能更愿意切换到手动选项，这样计算机就不会出现高 CPU 使用率，并避免每次修改场景时都会反复重新启动光照预计算。

手动启动预计算时，系统将评估和计算场景光照的所有方面。要自行重新计算并仅烘焙[反射探针](class-ReflectionProbe.html)，请单击 __Generate Lighting__ 按钮（菜单：__Window__ > __Lighting > Settings > Scene > Generate Lighting__）附带的下拉菜单，然后选择 __Bake Reflection Probes__。

**注意：**使用 __Auto Generate__ 模式时，Unity 会将您的光照数据存储在有限大小的临时缓存中。这意味着，当超出缓存的大小时，Unity 会删除旧的光照数据。如果某些场景依赖于已删除的自动生成的光照数据，则在构建项目时可能会出现问题。在这种情况下，您的场景可能在构建的项目中没有正确的光照。因此，在构建游戏之前，应取消选中 __Auto Generate__，并手动为所有场景生成光照数据。Unity 随后会将您的光照数据作为资源文件保存在项目文件夹中，这意味着数据将保存为项目的一部分并包含在您的构建版本中。

##启用 Baked GI 或 Realtime GI

Unity 在默认情况下启用 __Baked GI__ 和 __Realtime GI__。__Baked GI__ 全部预先计算好；__Realtime GI__ 在使用间接光照的情况下会执行一些预计算。启用这两个选项后，即可使用场景中的每个光源来控制应使用的 GI 系统（在光源组件中应使用 __Mode__ 设置来执行此操作）。请参阅有关 [Lighting 窗口](GlobalIllumination.html)和[全局光照](GIIntro.html)的文档以了解更多信息。

使用光照系统最灵活的方式是将 __Baked GI__ 和 __Realtime GI__ 一起使用。然而，这也是性能成本最高的选择。为了减少游戏的资源消耗，可选择禁用 __Realtime GI__ 或 __Baked GI__。请注意，这样做会降低光照系统的灵活性和功能。

要手动启用或禁用全局光照，请打开 Lighting 窗口 (__Window__ > __Lighting__ > __Settings__ > __Scene__)。勾选 __Realtime Global Illumination__ 可启用 __Realtime GI__，而勾选 __Baked Global Illumination__ 可启用 __Baked GI__。取消选中这些复选框可禁用相应的 GI 系统。如果任何光源设置为您已禁用的模式，它们将被覆盖并设置为激活的 GI 系统。

##每个光源的设置

要设置每个光源的属性，请在 Scene 或 Hierarchy 窗口中选择该光源，然后在 Inspector 窗口中编辑该[光源组件](class-Light.html)的设置。

每个光源的默认 __Mode__ 设置为 __Dynamic__。这意味着该光源为场景提供直接光照，并由 Unity 的 __Realtime GI__ 处理间接光照。

如果光源的 __Mode__ 设置为 __Static__，则该光源仅为 Unity 的 __Baked GI__ 系统提供光照。这些光源的直接光照和间接光照都被烘焙为光照贴图，并且在游戏过程中无法更改此设置。

如果光源的 __Mode__ 设置为 __Stationary__，则标记为 __Static__ 的游戏对象仍然会在其 __Baked GI__ 光照贴图中包含此光源。但是，与标记为 __Static__ 的光源不同，__Stationary__ 光源仍会根据 Lighting 窗口中的静止烘焙模式（菜单：__Window__ > __Lighting__ > __Settings__）提供实时光照。如果您在静态环境中使用光照贴图，这会很有用，但您仍希望在动态和光照贴图静态几何体之间进行良好的集成。

![__Mode__ 设置为 __Dynamic__ 的方向光。](../uploads/GlobalIllumination/LightBakingType.png)

请参阅有关[光照模式](https://docs.google.com/document/d/116JvLXljfbdfllOLlyzVvWmNWpbUwcYKV16blVHuS2E/edit?usp=sharing)的文档以了解更多详细信息。

##GI 缓存

在 Baked GI 或 Precomputed Realtime GI 中，Unity 会在“GI 缓存”(GI Cache) 中缓存（存储）有关场景光照的数据，并会尽可能重复使用这些数据，以便在预计算期间节省时间。您对场景所做更改的数量和性质将决定可以重复使用的数据量（如果有的话）。

此缓存会存储在 Unity 项目之外，可使用 (__Preference__ > __GI Cache__ > __Clear Cache__) 将其清除。清除此缓存意味着需要从头开始重新计算预计算的所有阶段，因此可能会很耗时。但是，在某些情况下，您可能需要减少磁盘使用量，此操作可能就会有所帮助。

##烘焙 GI 的 LOD（细节级别）

Unity 在生成烘焙光照贴图时会考虑细节级别。直接光照是用所有 LOD 的实际表面进行计算的。较低的 LOD 级别使用光照探针来获取间接光照。生成的光照将烘焙到光照贴图中。

这意味着，您应该在 LOD 周围放置光照探针以捕获间接光照。如果您使用完全烘焙的 GI，则对象在运行时不会使用光照探针。

---

* <span class="page-edit"> 2017-06-08  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 5.6 版中添加了“光照模式”</span>
