# 模型导入工作流程

**注意：**此工作流假定已经有需要导入的模型文件。如果还没有文件，可先阅读[有关如何导出 FBX 文件的指南](HOWTO-exportFBX.html)，再从 3D 建模软件导出文件。有关如何从 3D 建模软件导出*人形 (Humanoid)* 动画的指南，请参阅[人形资源准备](UsingHumanoidChars.html)。

模型文件可包含各种数据，例如角色网格、动画骨架和剪辑以及材质和纹理。最有可能的情况是，文件未同时包含所有这些元素，但可以根据需要按照本工作流程的任何部分进行操作。

**访问 Import Settings 窗口**


无论要从模型文件中提取哪种数据，始终以相同的方式开始：
1.打开 __Project__ 窗口和 __Inspector__，确保能同时看到这两个界面。
2.从 __Project__ 窗口的 __Asset__ 文件夹中选择要导入的__模型__文件。__Inspector__ 中将打开 __Import Settings__ 窗口，其中默认显示 __Model 选项卡__。


**设置模型特有的选项和常规导入器选项**

随后可在 [Model 选项卡](FBXImporter-Model.html)上设置这些选项：

* 使用 __Scale Factor__、__Use File Scale__ 或 __File Scale__ 属性可调整 Unity 对单位的解释。例如，3ds Max 使用 1 个单位表示 10 厘米，而 Unity 使用 1 个单位表示 1 米。
* 使用 __Mesh Compression__、__Read/Write Enabled__、__Optimize Mesh__、__Keep Quads__、__Index Format__ 或 __Weld Vertices__ 属性可减少资源并节省内存。
* 如果模型文件来自 Maya 或 3ds Max，或者任何其他支持变形目标动画的 3D 建模应用程序，则可以启用 __Import BlendShapes__ 选项。
* 可为环境几何形状启用 __Generate Colliders__ 选项。
* 可启用特定的 FBX 设置，如 __Import Visibility__ 或 __Import Cameras__ 和 __Import Lights__。
* 对于仅包含动画的模型文件，可启用 __Preserve Hierarchy__ 选项以防止骨架层级视图不匹配。
* 如果使用了__光照贴图 (Lightmap)__，则可设置 __Swap UVs__ 和 __Generate Lightmap UVs__。
* 可使用 __Normals__、__Normals Mode__、__Tangents__ 或 __Smoothing Angle__ 选项来控制 Unity 如何处理模型中的法线 (Normals) 和切线 (Tangents)。


**设置骨架和动画的导入选项**

如果文件包含动画数据，可遵循使用 [Rig 选项卡](FBXImporter-Rig.html)设置骨架并使用 [Animation 选项卡](class-AnimationClip.html)提取或定义动画剪辑的准则。人形和通用（非人形）动画类型的工作流程不同，因为 Unity 要求人形类型有非常具体的骨骼结构，但对于通用类型只需了解哪个骨骼是根节点：

* [人形类型工作流程](ConfiguringtheAvatar.html)
* [通用类型工作流程](GenericAnimations.html)


**处理材质和纹理**

如果文件包含__材质__或__纹理__，可定义它们的处理方式：

1.单击 __Import Settings__ 窗口中的 [Materials 选项卡](FBXImporter-Materials.html)。
2.启用 __Import Materials__ 选项。__Materials__ 选项卡中显示多个选项，包括 __Location__ 选项，该选项的值决定了应显示的其他选项。
3.选择 __Use Embedded Materials__ 选项[将导入的材质保持在导入的资源中](FBXImporter-Materials.html#Embedded)。

设置完这些选项后，请单击 __Import Settings__ 窗口底部的 __Apply__ 按钮来保存这些设置，也可单击 __Revert__ 按钮取消设置。

最后，可将文件导入场景中：

* 如果文件中包含网格，请将文件拖入 __Scene__ 视图以将其实例化为__游戏对象__的__预制件__。
* 如果文件中包含动画剪辑，可将文件拖入 [Animator 窗口](AnimatorWindow.html)以便在[状态机](StateMachineBasics.html)中使用，或拖到 [Timeline 窗口](TimelineEditorWindow.html)的__动画轨道 (Animation track)__ 上。也可以将动画连续片段直接拖动到 Scene 视图中的实例化预制件上。此操作将自动创建一个动画控制器 (Animation Controller) 并将动画连接到模型。

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
