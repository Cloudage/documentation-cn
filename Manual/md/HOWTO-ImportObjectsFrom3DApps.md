# 从其他应用程序导入时的限制

Unity 导入专有文件时，会在后台启动 3D 建模软件。Unity 随后与该专有软件通信，将原生文件转换为 Unity 可读取的格式。

首次将专有文件导入 Unity 时，3D 建模软件必须在命令行进程中启动。此过程可能需要一段时间，但后续导入操作非常快。

**警告：**建议[导出 FBX](HOWTO-exportFBX.html)，而不是直接保存为项目应用中的默认格式。建议不要在生产中直接使用原生文件格式。

## 要求

必须安装 3D 建模软件才能将专有文件直接导入 Unity。
如果未安装该软件，请改用 FBX 格式。
有关导入 FBX 文件的更多信息，请参阅 [Model Import Settings 窗口](class-FBXImporter.html)。

## 应用程序特有的问题

无论文件是通用文件还是特定文件，均以相同方式[导入文件](class-FBXImporter.html)。但是，在具体支持哪些功能方面存在一些差异。有关特定 3D 应用程序的限制的更多信息，请参阅：

* [从 Maya 导入对象](#Maya)
* [从 Cinema 4D 导入对象](#Cinema4D)
* [从 3ds Max 导入对象](#Max)
* [从 Cheetah3D 导入对象](#Cheetah3D)
* [从 Modo 导入对象](#Modo)
* [从 LightWave 导入对象](#Lightwave)
* [从 Blender 导入对象](#Blender)
* [SketchUp 设置](HOWTO-ImportObjectSketchUp.html)


<a name="Maya"></a> 
## 从 Maya 导入对象

Unity 通过 FBX 格式导入 Maya 文件（*.mb* 和 *.ma*），支持以下内容：

* 所有节点以及位置、旋转和缩放；轴心点和名称也会导入
* 网格以及顶点颜色、法线和最多 2 个 UV 集
* 材质以及纹理和漫射颜色；每个网格多种材质
* 动画
* 关节
* Blendshape
* 光照和摄像机
* 可见性
* 自定义属性动画

*提示*：有关如何从 Maya 导出 FBX 文件的信息，请参阅[从其他应用程序导出](HOWTO-exportFBX.html)。

### 限制

Unity 不支持 Maya 的_旋转轴 (Rotate Axis)_（旋转前）。

关节限制包括：

* _关节方向 (Joint Orient)_（仅限关节旋转后）
* _分段缩放补偿 (Segment Scale Compensate)_（仅限关节选项）

Unity 可导入和支持 Maya 中指定的任何_旋转顺序 (Rotate Order)_；但是一旦导入，便无法在 Unity 中更改该顺序。
如果导入的模型使用的旋转顺序不同于 Unity 中的旋转顺序，Unity 会在 __Inspector__ 中的 __Rotation__ 属性旁边[显示该旋转顺序](AnimationEulerCurveImport.html#RotationOrder)。

### 提示和故障排除

* 保持场景轻量级：导出时仅导出 Unity 需要使用的对象。
* Unity 仅支持多边形，因此在导出前应将所有面片或 NURBS 表面转换为多边形；请参阅 [Maya 文档](https://knowledge.autodesk.com/support/maya/learn-explore/caas/CloudHelp/cloudhelp/2015/ENU/Maya/files/Polygon-selection-and-creation-Convert-NURBS-surfaces-to-a-polygon-mesh-htm.html)了解相关说明。
* 如果模型未正确导出，可能是 Maya 中的节点历史记录导致的问题。在 Maya 中，选择 **Edit** &gt; **Delete by Type** &gt; **Non-Deformer History**，然后重新导出模型。
* Maya FBX Exporter 会烘焙不支持的复杂动画约束，例如 Set Driven Keys（设置受驱动关键点），从而将动画正确导入 Unity。如果在 Maya 中使用了 Set Driven Keys，确保在驱动者 (driver) 上设置关键点，以便正确烘焙动画。有关更多信息，请参阅 Maya 提供的关键帧动画 (Keyframe Animation) 文档。
* 在 Maya 中，可见性值呈现在各个形状上，但不能添加动画，也不会导出为 FBX 格式文件。必须在节点上而不是在形状上设置可见性值。


<a name="Cinema4D"></a> 
## 从 Cinema 4D 导入对象

Unity 通过 FBX 格式导入 Cinema 4D 文件 (*.c4d*)，支持以下内容：

 * 所有对象以及位置、旋转和缩放；轴心点和名称也会导入
 * 网格以及 UV 和法线
 * 材质以及纹理和漫射颜色；每个网格多种材质
 * 动画正向动力学 (FK)（IK（反向动力学）需手动烘焙）
 * 基于骨骼的动画

*提示*：有关如何从 Cinema 4D 导出 FBX 文件的信息，请参阅[从其他应用程序导出](HOWTO-exportFBX.html)。


### 限制

Unity 不会导入 Cinema 4D 的点级动画 (Point Level Animations, PLA)。请改用基于骨骼的动画。

Cinema 4D 不会导出可见性继承。在 Cinema 4D 中将 Renderer 设置为“Default”或“Off”以避免 Cinema4D 和 Unity 之间的可见性动画存在任何差异。



<a name="Max"></a> 
## 从 3ds Max 导入对象

Unity 通过 FBX 格式导入 3ds Max 文件 (*.max*)，支持以下内容：

* 所有节点以及位置、旋转和缩放；轴心点和名称也会导入
* 网格以及顶点颜色、法线和一个或多个 UV 集
* 材质以及漫射纹理和颜色。每个网格多种材质
* 动画
* 基于骨骼的动画
* 变形 (Blendshape)
* 可见性

**注意**：保存 3ds Max 文件 (.max) 或导出通用 3D 文件类型 (.fbx) 各有优缺点，请参阅 [class-Mesh](class-Mesh.html)。

*提示*：有关如何从 3ds Max 导出 FBX 文件的信息，请参阅[从其他应用程序导出](HOWTO-exportFBX.html)。



<a name="Cheetah3D"></a> 
## 从 Cheetah3D 导入对象

Unity 通过 FBX 格式导入 Cheetah3D 文件 (*.jas*)，支持以下内容：

* 所有节点以及位置、旋转和缩放；轴心点和名称也会导入
* 网格以及顶点、多边形、三角形、UV 和法线
* 动画
* 材质以及漫射颜色和纹理

*提示*：有关如何从 Cheetah3D 导出 FBX 文件的信息，请参阅[从其他应用程序导出](HOWTO-exportFBX.html)。



<a name="Modo"></a> 
## 从 Modo 导入对象

Unity 通过 FBX 格式导入 Modo 文件 (*.lxo*)，支持以下内容：

* 所有节点以及位置、旋转和缩放；轴心点和名称也会导入
* 网格以及顶点、法线和 UV。
* 材质以及纹理和漫射颜色；每个网格多种材质
* 动画

开始前，请将 **.lxo** 文件保存到项目的 Assets 文件夹。在 Unity 中，该文件显示在项目视图中。

Unity 检测到 .lxo 文件变化后会重新导入资源。

*提示*：有关如何从 Modo 导出 FBX 文件的信息，请参阅[从其他应用程序导出](HOWTO-exportFBX.html)。



<a name="Lightwave"></a> 
## 从 Lightwave 导入对象

Unity 通过 FBX 格式导入 Lightwave 文件，支持以下内容：

 * 所有节点以及位置、旋转和缩放；轴心点和名称也会导入
 * 网格以及最多 2 个 UV 通道
 * 法线
 * 材质以及纹理和漫射颜色；每个网格多种材质
 * 动画
 * 基于骨骼的动画

还可以配置 Lightwave AppLink 插件，该插件会自动保存第一次将 Lightwave 场景文件导入 Unity 时使用的 FBX 导出设置。
有关更多信息，请参阅 [Lightwave Unity 互换文档](https://docs.lightwave3d.com/display/LW2018/Unity)。

*提示*：有关如何从 Lightwave 文件导出 FBX 文件的信息，请参阅[从其他应用程序导出](HOWTO-exportFBX.html)。

### 限制

应将 Lightwave 特有的材质烘焙为纹理，以便 Unity 可以读取它们。有关使用非破坏性管线执行此操作的信息，请参阅 [Lightwave 中的节点系统](https://www.lightwave3d.com/learn/article/using-lightwave-to-create-uv-maps-for-military-assets/)。

Unity 不支持样条曲线和面片。必须将所有样条曲线和面片转换为多边形后才能保存和导出到 Unity。有关更多信息，请参阅 [Lightwave 文档](https://docs.lightwave3d.com/display/LW2018/Patch)。



<a name="Blender"></a> 
## 从 Blender 导入对象

Unity 通过 FBX 格式导入 [Blender](https://docs.blender.org/) (*.blend*) 文件，支持以下内容：

* 所有节点以及位置、旋转和缩放；轴心点和名称也会导入
* 网格以及顶点、多边形、三角形、UV 和法线
* 骨骼
* 蒙皮网格
* 动画

如需了解如何以最佳方式将 Blender 文件导入 Unity，请参阅[从其他应用程序导出](HOWTO-exportFBX.html)。

### 限制

纹理和漫射颜色不会自动分配。将纹理拖动到 Unity __Scene 视图__中的网格上来手动分配它们。

Blender 不会在 FBX 文件中导出动画内的可见性值。

---

* <span class="page-edit"> 2018-04-25  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
