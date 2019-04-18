# 资源审核

实际项目中发现的许多问题都是源自无心之过：临时的“测试”更改和疲惫不堪的开发人员的误点击可能会暗地里添加性能不良的资源或更改现有资源的导入设置。

对于任何大规模的项目，最好是将防止人为错误作为第一道防线。编写一小段代码来禁止将 4K 未压缩纹理添加到项目中，是相对简单的事情。

但是，这种错误操作却是十分常见的。一个 4K 的未压缩纹理会占用 60 MB 的内存。在低端移动设备（如 iPhone 4S）上，占用的内存超过大约 180-200 MB 是十分危险的。如果误添加此类纹理，就会无意中占用应用程序内存预算的三分之一到四分之一，并导致难以诊断的内存不足错误。

虽然现在可以使用 5.3 版内存性能分析器来跟踪这些问题，但最好在最初就确保这些错误不可能发生。

## 使用 AssetPostprocessor

Unity Editor 中的 `AssetPostprocessor` 类可用于在 Unity 项目上强制执行某些最低标准。导入资源时将回调此类。要使用此类，应继承 `AssetPostprocessor` 并实现一个或多个 `OnPreprocess` 方法。重要的方法包括：

* `OnPreprocessTexture`

* `OnPreprocessModel`

* `OnPreprocessAnimation`

* `OnPreprocessAudio`

请参阅 [AssetPostprocessor](../ScriptReference/AssetPostprocessor.html) 的脚本参考，了解更多可能的 `OnPreprocess` 方法。

```

public class ReadOnlyModelPostprocessor : AssetPostprocessor {

   public void OnPreprocessModel() {

        ModelImporter modelImporter =

 (ModelImporter)assetImporter;

        if(modelImporter.isReadable) {

            modelImporter.isReadable = false;

            modelImporter.SaveAndReimport();

        }

    }

} 

```

这是一个在项目中 `AssetPostprocessor` 强制执行规则的简单示例：

每次将模型导入项目时，或者模型的导入设置发生更改时，都会调用此类。该代码只是检查 `Read/Write enabled` 标志（由 `isReadable` 属性表示）是否设置为 `true`。如果是，则强制将标志位更改为 `false`，然后保存并重新导入资源。

请注意，调用 `SaveAndReimport` 会导致再次调用此代码片段！但是，因为现在已确保 `isReadable` 为 false，所以此代码不会产生无限的重新导入循环。

此变更的原因将在下面的“模型”部分讨论。

##通用资源规则

#### 纹理

**禁用 read/write enabled 标志**

`Read/Write enabled` 标志使纹理在内存中保留两次：一次保存在 GPU 中，一次保存在 CPU 可寻址内存中(1)（注意：
	 这是因为大多数平台上从 GPU 内存回读的速度极慢。将纹理从 GPU 内存读入临时缓冲区以供 CPU 代码（例如 Texture.GetPixel）使用将是非常低效的）。在 Unity 中，默认情况下禁用此设置，但可能会无意中将其打开。

只有在着色器之外操作纹理数据时（例如使用 `Texture.GetPixel` 和 `Texture.SetPixel` API 时）才需要 `Read/Write Enabled`，否则应尽可能避免使用它。

**尽可能禁用 Mipmap**

如果对象相对于摄像机具有相对不变的 Z 深度，则可禁用 Mipmap，这样将大约节省加载纹理所需的内存的三分之一。如果对象的 Z 深度会发生变更，则禁用 Mipmap 可能导致 GPU 上的纹理采样性能变差。

通常情况下，这对于 UI 纹理以及在屏幕上以恒定大小显示的其他纹理非常有用。

**压缩所有纹理**

使用适合项目目标平台的纹理压缩格式对于节省内存至关重要。

如果所选的纹理压缩格式不适合目标平台，Unity 会在加载纹理时解压缩纹理，这将消耗 CPU 时间和额外的内存。此问题在 Android 设备上最常见，因为此类平台通常因芯片组不同而支持截然不同的纹理压缩格式。

**实施合理的纹理大小限制**

虽然很简单，但也很容易忘记调整纹理大小或无意中更改纹理大小导入设置。应确定不同类型纹理的合理最大值，并通过代码强制执行这些限制规则。

对于许多移动应用程序，2048x2048 或 1024x1024 足以满足纹理图集的要求，而 512x512 足以满足应用于 3D 模型的纹理的要求。

#### 模型

**禁用 Read/Write enabled 标志**

模型的 `Read/Write enabled` 标志与纹理的上述相应标志具有相同的工作原理。但是，模型在默认情况下会启用该标志。

如果项目在运行时通过脚本修改网格 (Mesh)，或者如果网格用作 MeshCollider 组件的基础，则 Unity 会要求启用此标志。如果模型未在 MeshCollider 中使用并且未被脚本操纵，请禁用此标志以节省一半模型内存。

**在非角色模型上禁用骨架**

默认情况下，Unity 会为非角色模型导入通用骨架。如果模型在运行时实例化，则会导致添加 `Animator` 组件。如果模型没有通过动画系统进行动画处理，则会给动画系统增加不必要的开销，因为每帧都必须运行一次所有激活的 Animator。

在非动画模型上禁用骨架可以避免自动添加 Animator 组件，并防止可能无意中向场景添加不需要的 Animator。

**在动画模型上启用 Optimize Game Objects 选项**

__Optimize Game Objects__ 选项对动画模型有着显著的性能影响。禁用该选项后，Unity 会在每次实例化模型时创建一个大型变换层级视图来镜像模型的骨骼结构。此变换层级视图的更新成本很高，尤其是在附加了其他组件（如粒子系统或碰撞体）的情况下。它还限制了 Unity 通过多线程执行网格蒙皮和骨骼动画计算的能力。

如果需要暴露模型骨骼结构上的特定位置（例如暴露模型的双手以便动态附加武器模型），则可在 `Extra Transforms` 列表中将这些位置专门列入白名单。

如需了解更多详细信息，请参阅 Unity 手册的 [Model Importer](https://docs.unity3d.com/Manual/FBXImporter-Rig.html) 页面。

**尽可能使用网格压缩**

启用网格压缩可减少用于表示模型数据不同通道的浮点数的位数。这样做可能导致精确度的轻微损失，在用于最终项目之前，美术师应评估这种不精确性的影响。

在给定压缩级别中使用的具体位数在 [ModelImporterMeshCompression](https://docs.unity3d.com/ScriptReference/ModelImporterMeshCompression.html) 脚本参考中有详细说明。

请注意，可对不同的通道使用不同级别的压缩，因此项目可选择仅压缩切线和法线，同时保持 UV 和顶点位置不压缩。

**注意网格渲染器设置**

将网格渲染器添加到预制件或游戏对象时，请注意组件上的设置。默认情况下，Unity 会启用阴影投射和接收、光照探针采样、反射探针采样和运动矢量计算。

如果项目不需要这些功能中的一个或多个，请确保通过自动脚本关闭它们。添加网格渲染器的任何运行时代码也都需要处理这些设置。

对于 2D 游戏，在启用阴影选项的情况下意外地将网格渲染器添加到场景会为渲染循环添加完整的阴影 pass。通常情况下，这是对性能的浪费。

#### 音频

**适合平台的压缩设置**

应为音频启用与可用硬件匹配的压缩格式。所有 iOS 设备都包含硬件 MP3 解压器，而许多 Android 设备本身支持 Vorbis。

此外，应将未压缩的音频文件导入 Unity。Unity 在构建项目时总是会重新压缩音频。无需导入压缩的音频再重新压缩，这只会降低最终音频剪辑的质量。

**将音频剪辑强制设置为单声道**

很少有移动设备实际配备立体声扬声器。在移动平台项目中，将导入的音频剪辑强制设置为单声道会使其内存消耗减半。此设置也适用于没有立体声效果的任何音频，例如大多数 UI 声音效果。

**降低音频比特率**

尽量降低音频文件的比特率，以进一步节省内存消耗和构建的项目大小，但这种情况需要咨询音频设计师。
