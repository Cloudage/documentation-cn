# 故障排除

本部分介绍使用 Unity 时可能出现的常见问题。每个平台在下面单独说明。

## 特定于平台的故障排除

### OSX 10.6.4 上的 Geforce 7300GT

由于 OX 10.6.4 上的 Geforce 7300GT 无法正确显示材质，延迟渲染被禁用；这是因为视频驱动程序有错误。

### 在 Windows x64 上，当脚本抛出 NullReferenceException 时，Unity 崩溃

需要应用 [Windows 修补程序 #976038](http://support.microsoft.com/kb/976038)。

## 脚本编辑

### 即使将 Visual Studio 设置为脚本编辑器，也会在默认系统文本编辑器中打开脚本

当 Visual Studio 报告无法打开脚本时会发生这种情况。最常见的原因是外部插件（如 Resharper）在启动时显示对话框，请求用户提供输入。这会导致 Visual Studio 报告无法打开问题。

## 图形

###帧率很慢和/或视觉瑕疵

如果视频卡驱动程序不是最新版本，可能会发生这种情况。确保已安装视频卡供应商提供的最新官方驱动程序。

## 阴影

* 阴影需要一定的图形硬件支持。有关详细信息，请参阅[光照性能](LightPerformance.html)页面。
* 确保在 [Quality Settings](class-QualitySettings.html) 中启用阴影。
* Android 和 iOS 上的阴影有局限性：柔和阴影不可用，而在前向渲染路径中，只有单个方向光可以投射阴影。在延迟渲染路径中，投射阴影的光源数量没有限制。

### 某些游戏对象不投射或接受阴影

对象的[渲染器 (Renderer)](class-MeshRenderer.html) 必须启用 __Receive Shadows__ 选项才能在对象上渲染阴影。此外，对象必须启用 __Cast Shadows__ 选项才能在其他对象上投射阴影（这两个选项都是默认打开的）。

只有不透明对象才能投射和接受阴影。这意味着使用内置[透明](shader-TransparentFamily.html)着色器或粒子着色器的对象不会投射阴影。在大多数情况下，可以对栅栏、植被等对象使用[透明镂空](shader-TransparentCutoutFamily.html)着色器。如果使用自定义编写的[着色器](Shaders.html)，这些着色器必须采用像素光照并使用[几何渲染队列](SL-SubShaderTags.html)。使用__顶点光照 (VertexLit)__ 着色器的对象不能接受阴影，但可以投射阴影。

只有__像素光照__会投射阴影。如果想确保光源始终投射阴影，而不管场景中有多少其他光源，那么可以将其设置为 __Force Pixel__ 渲染模式（请参阅[光源](class-Light.html)参考页面）。
