# 模型文件格式

Unity 支持从两种不同类型的文件导入[网格](class-Mesh.html)和[动画](AnimationClips.html)：

* [导出的 3D 文件格式](#Exported3DFiles)，如 .fbx 或 .obj。您可以使用通用格式从 3D 建模软件导出文件，此类格式的文件可以由各种不同的软件导入和编辑。

* [专有的 3D 或 DCC（数字内容创作）应用程序文件](#Proprietary3DappFiles)，例如来自 [3D Studio Max](https://www.autodesk.com/products/3ds-max/overview) 或 [Blender](https://www.blender.org/) 的 .max 和 .blend 文件格式。只能在创建专有文件的软件中编辑这些文件。专有文件通常无法在未经转换和导入的情况下直接由其他软件编辑。但 [SketchUp](https://www.sketchup.com/) .skp 文件是一个例外；SketchUp 和 Unity 均可读取此格式的文件。

Unity 可以导入和使用这两种类型的文件，每种文件都有各自的优缺点。


<a name="Exported3DFiles"></a> 
## 导出的 3D 格式

Unity 可读取 [.fbx](https://www.autodesk.com/products/fbx/overview)、[.dae (Collada)](https://www.khronos.org/collada/)、.3ds、.dxf、.obj 和 .skp 文件。有关导出 3D 文件的信息，请参阅[从其他应用程序导出](HOWTO-exportFBX.html)或阅读 3D 建模软件的文档。

**优点：**

* 可以只导入所需的模型部分，而无需将整个模型导入 Unity。
* 导出的通用文件通常小于专有的同等文件。
* 使用导出的通用文件有利于采用模块化方法（例如，为碰撞类型或交互使用不同的组件）。
* 可以从 Unity 不直接支持的软件导入这些文件。
* 可以将已导出的 3D 文件（.fbx、.obj）重新导入 3D 建模软件，以此确保所有信息都已正确导出。

**缺点：**

* 如果原始文件发生更改，则必须手动重新导入模型。
* 需要跟踪源文件和导入 Unity 的文件之间的版本。


<a name="Proprietary3DappFiles"></a> 
## 专用的 3D 应用程序文件

Unity 可以从以下 3D 建模软件导入专有文件：

* [3D Studio Max](https://www.autodesk.com/products/3ds-max/overview)
* [Maya](https://www.autodesk.com/products/maya/overview)
* [Blender](https://www.blender.org/)
* [Cinema4D](https://www.maxon.net/en/products/cinema-4d/overview/)
* [Modo](https://www.foundry.com/products/modo)
* [LightWave](https://www.lightwave3d.com/)
* [Cheetah3D](https://www.cheetah3d.com/)

**警告：**Unity 在导入过程中将专有文件转换为 .fbx 文件。但是，建议导出 FBX，而不是直接保存为工程应用中的默认格式。建议不要在生产中直接使用原生文件格式。

**优点：**

* 如果原始模型更改，Unity 会自动重新导入文件。
* 这最初很简单；但是在开发后期会变得更复杂。

**缺点：**

* 必须在使用 Unity 项目的每台计算机上安装所用软件的授权副本。
* 使用 Unity 项目的每台计算机上的软件版本应相同。使用不同的软件版本可能会在导入 3D 模型时导致错误或意外行为。
* 文件可能会因不必要的数据而变得臃肿。
* 大文件可能会降低 Unity 项目导入或资源重新导入的速度，因为在将模型导入 Unity 时必须运行 3D 建模软件作为后台进程。
* 在导入过程中，Unity 会在内部将专有文件导出到 .fbx。因此难以验证 .fbx 数据和进行故障排除。

***注意：***除非在计算机上安装了相应的 3D 建模软件，否则保存为 .ma、.mb、.max、.c4d 或 .blend 文件的资源将无法导入。这意味着，处理 Unity 项目的每个人都必须安装正确的软件。例如，如果您使用 [Maya LT 许可证](https://www.autodesk.com/products/maya-lt/overview)来创建 `ExampleModel.mb` 并将其复制到项目中，那么任何打开该项目的用户也需要在他们的计算机上安装 Maya LT。

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
