#升级到 Unity 5.4

将项目从 Unity 5.3 升级到 Unity 5.4 时，应了解可能会影响现有项目的一些更改。


##网络：Multiplayer 服务 API 更改

[对网络 API 的大量更改](UpgradeGuide54Networking.html)。

##网络：WebRequest 不再是实验性的

`WebRequest` 接口已从 `UnityEngine.Experimental.Networking` 升级为 `UnityEngine.Networking`。必须更新使用了 `UnityWebRequest` 的 Unity 5.2 和 5.3 项目。

##Scene 视图：不自动应用色调映射

具有 `ImageEffectTransformsToLDR` 属性的图像效果将不再直接应用于 Scene 视图（如果存在）。现在有一个用于将效果应用于 Scene 视图的新属性：`ImageEffectAllowedInSceneView`。5.4 版标准资源已升级来反映这一变化。

##着色器：已重命名变量

为了保持一致性，已重命名许多内置着色器变量：

- **\_Object2World** 和 **\_World2Object** 现在重命名为 **unity_ObjectToWorld** 和 **unity_WorldToObject**
- **\_World2Shadow** 现在重命名为 **unity_WorldToShadow[0]**，**\_World2Shadow1** 重命名为 **unity_WorldToShadow[1]**，以此类推
- **\_LightMatrix0** 现在重命名为 **unity_WorldToLight**
- **\_WorldToCamera**、**\_CameraToWorld**、**\_Projector**、**\_ProjectorDistance**、**\_ProjectorClip** 和 **\_GUIClipTextureMatrix** 先前全都添加前缀 **unity**

在导入 .shader 和 .cginc 文件时，其中的变量引用将自动重命名。但是，导入后，如果不手动重命名变量，着色器将无法在 Unity 5.3 或更早版本中使用。

##着色器：一致数组

在 Unity 5.4 中，处理着色器属性数组的方式已发生变化。现在，针对着色器中的浮点/矢量/矩阵数组，提供了“原生”支持（通过 `MaterialPropertyBlock.SetFloatArray`、`Shader.SetGlobalFloatArray` 等）。这些新 API 支持包含最多 1,023 个元素的数组。

在 `Material` 和 `MaterialPropertyBlock` 中都已弃用以数字为后缀的名称来引用单个数组元素的旧方法（例如 `_Colors0`、`_Colors1`）。使用 `Material` 序列化的这类属性不再能够设置数组元素（但如果任何一致数组的名称后缀为数字，此方法仍然有效）。

## 着色器：5.4 版中的其他更改

默认着色器编译目标已更改为“#pragma target 2.5”（WinPhone 上的 DX9 SM3.0、DX11 9.3 功能级别）。仍然可以使用“#pragma target 2.0”将目标设置为 DX9 SM2.0 和 DX11 9.1 功能级别。

现在大多数内置着色器的目标是 2.5：明显的例外是 Unlit、VertexLit 和固定函数着色器。实际上，这意味着，默认情况下大多数内置着色器和新创建的着色器将无法在 2004 年之前生产的 PC GPU 上运行。请参阅[此博客文章](http://blogs.unity3d.com/2015/08/27/plans-for-graphics-features-deprecation/)了解详细信息。

已弃用的 `Material` 类构造函数 `Material(string)` 在 5.4 版中停止工作。使用它将输出错误并导致洋红色的错误着色器。

用于计算屏幕空间方向光阴影的内部着色器已移至图形设置 (Graphics Settings)。如果先前通过在项目中使用该着色器的副本来使用方向光阴影的自定义版本，现在将无法再使用。现在应在 __Edit > Project Settings > Graphics__ 下选择自定义着色器。

反射探针在两个纹理之间共享一个采样器。如果在着色器中手动对它们进行采样，并得到一个“undeclared identifier samplerunity_SpecCube1”错误，则需要将代码从 `UNITY_PASS_TEXCUBE(unity_SpecCube1)` 更改为 `UNITY_PASS_TEXCUBE_SAMPLER(unity_SpecCube1,unity_SpecCube0)`。

`UnityEditor.ShaderUtil.ShaderPropertyTexDim` 已弃用；请改用 `Texture.dimension`。

##ComputeBuffer

自动转换的 OpenGL 着色器中的 ComputeBuffer 数据布局已更改为与 DirectX ComputeBuffer 的布局相匹配。如果在 OpenGL 中使用 ComputeBuffer，请删除所有用于调整数据来匹配以前 OpenGL 特定布局规则的相关代码。请参阅[计算着色器](ComputeShaders.html)以了解更多信息。

##Playable：迁移到 5.4

* Playable 现在是结构而不是类。

* `Playable` 结构是原生 `Playable` 类的句柄，而不是指向原生 `Playable` 类的指针。

* 非 null 的 `Playable` 结构不保证 Playable 可用。请使用 `.IsValid` 方法验证 Playable 是否可用。

* 用于为空输入/输出返回 null 的所有方法现在将返回 `Playable.Null`。

* `Playable.Null` 是无效的 Playable。

* 可将 `Playable.Null` 传递给 `AddInput`、`SetInput` 和 `SetInputs` 来保留空输入或用于隐式断开连接的输入。

* 如果使用 `Playable.Null` 或任何无效的 Playable 作为任何方法的输入，或者对无效的 Playable 调用方法，则会为该操作抛出相应的异常。

* 将 Playable 与 `null` 进行比较现在毫无意义。应与 `Playable.Null` 比较。

* 必须使用希望使用的类的 `Create` 静态方法来分配 Playable。

* 必须对 Playable 句柄使用 `.Destroy` 方法来取消分配 Playable。

* 未取消分配的 Playable 将泄漏。

* Playable 已转换为结构以避免装箱/拆箱，从而提高性能（[有关装箱的更多信息](https://msdn.microsoft.com/en-us/library/yz2be5wk.aspx)）。

* 隐式或显式将 `Playable` 转换为对象将导致装箱/拆箱，从而降低性能。

* 从 Playable 类继承将导致子类的实例进行装箱/拆箱。

* 由于现在只有动画可用，`ScriptPlayable`s 已被 `CustomAnimationPlayables` 取代。

* 无法再从基础 Playable 派生。只需在自定义 Playable 中聚合 Playable。


##Oculus Rift：从 Unity 5.3 升级项目

请按照以下说明从 Unity 5.3 升级 Oculus VR 项目：

重新启用虚拟现实支持。

* 打开 [Player Settings](class-PlayerSettings.html)。（菜单：__Edit &gt; Project Settings &gt; Player__）
* 选择 __Other Settings__ 并选中 __Virtual Reality Supported__ 复选框。使用该复选框下方显示的 Virtual Reality SDK 列表为每个构建目标添加和删除虚拟现实设备。

删除 __Oculus Spatializer__。

* 通过 Audio Settings 窗口（菜单：__Edit &gt; Project Settings &gt; Audio__）使用 Spatializer Plugin 下拉选单从项目中删除 __Oculus Spatializer__ 音频插件。该插件可能与原生的空间音响功能冲突并阻止构建。

##对同级游戏对象重新排序

重新排序同级游戏对象时触发的事件在 Unity 5.4 中发生了变化。同级游戏对象是指在 Hierarchy 窗口中具有相同父对象的游戏对象。在 Unity 的早期版本中，更改同级游戏对象的顺序会导致每个同级对象都接收到一个 `OnTransformParentChanged` 调用。在 5.4 中，同级游戏对象不再接到此调用。取而代之的是父游戏对象接收到单个 `OnTransformChildrenChanged` 调用。

这意味着，如果项目中的代码依赖于在重新排序同级游戏对象时调用 __OnTransformParentChanged__，那么这些调用现在将不再发生，并需要更新代码以使父对象收到 __OnTransformChildrenChanged__ 调用时执行操作。

进行此更改的原因是新行为可以提高性能。

##在运行时重新排列大型游戏对象层级视图
由于变换组件的优化，如果层级视图包含 1000 个以上的游戏对象，则使用 `Transform.SetParent` 或 `Destroy`ing 操作可能需要很长时间。建议不要在运行时调用 `SetParent` 或通过其他方式重新排列如此大的层级视图。

##Windows 应用商店

已为所有 .NET 脚本后端 SDK 更新了生成的 Visual Studio 项目格式。这可以修复当生成的项目中没有任何更改时进行过多的重建。您可能需要删除现有生成的 *.csproj（尤其如果它是在选中“Generate C# projects”选项的情况下构建的），以便 Unity 可以重新生成它。

##脚本序列化错误

在反序列化（加载）期间从构造函数和字段初始化程序调用 Unity API 时，可捕获两个新的脚本序列化错误。反序列化可能发生在与主线程不同的线程上，因此在反序列化期间调用 Unity API 是不安全的。Unity 手册中的(脚本序列化)[script-Serialization]页面底部提供了更多详细信息。

##支持 Retina 屏幕

Editor 现在支持 Mac OS X 上的 Retina 分辨率，提供高分辨率文本、UI 和 3D 视图。

现在 Editor GUI 在点空间而不是像素空间中定义。在标准分辨率显示屏上，没有变化，因为每个点都是一个像素。然而，在 Retina 显示屏上，每个点都是两个像素。当前屏幕与 UI 比例可用作 `EditorGUIUtility.pixelsPerPoint`。由于 Unity 可以有多个窗口，每个窗口会显示在具有不同像素密度的监视器上，因此该值可能会在视图之间发生变化。

如果 Editor 代码使用常规的 Editor/GUI/Layout 方法，很可能不需要进行任何更改。

如果使用 `Screen.width/height`，请改用 `EditorWindow.position.width/height`。这是因为屏幕大小是以像素为单位测量的，但 UI 是以点为单位定义的。对于在 Inspector 中显示的自定义编辑器，请使用 `EditorGUIUtility.currentViewWidth`，它能正确反映滚动条的存在。

如果要在 UI 中显示其他内容（例如 RenderTexture），可能要以点为单位计算其大小。为了支持 Retina 分辨率，需要将点大小转换为像素大小。[EditorGUIUtility](../ScriptReference/EditorGUIUtility.html) 中有用于此目的的新方法。

如果使用具有自定义背景的 GUIStyle，则可以通过将具有恰好双倍尺寸的一个纹理放入“GUIStyleState.scaledBackgrounds”数组中来添加 Retina 版本的背景纹理。

由于分辨率提高，图形硬件不够强大的 Mac 可能在 3D 视图中出现不可接受的 Editor 帧率迟缓问题。可通过在 Finder 中的 Unity.app 上选择“Get Info”并选中“Open in Low Resolution”来禁用 Retina 支持。


##物理：网格和物理变换漂移


存在以下方面的更改：

* 如果物理网格包含无效（非有限）顶点，则会拒绝此类物理网格。
* 通过不发送冗余变换更新来避免物理变换漂移。

5.3 版的行为是动画系统始终为恒定的动画曲线发送变换更新消息。这些消息会唤醒刚体，而修复这个问题在 5.3 版中被证明是非常危险的。

在 5.4 版中，如果没有位置变化，刚体就不会唤醒（这符合大多数人的预期）。

如果项目依赖于一直唤醒刚体的 5.3 版行为，可能无法在 5.4 版中按预期运行。

##Web 播放器

已从 Unity 5.4 中删除 Unity Web 播放器目标。如果将项目升级到 5.4 版，则无法将其部署到 Web 播放器平台。

如果有需要维护的旧版 Web 播放器项目，请不要将它们升级到 5.4 或更高版本。
 
