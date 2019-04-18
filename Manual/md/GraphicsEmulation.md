#图形硬件功能和仿真


最终渲染场景的图形硬件由称为[着色器](class-Shader.html)的专用程序来控制。硬件的功能已在不停进步，伴随每个阶段产生的一般功能集称为 [Shader Model](SL-ShaderCompileTargets.html)。不断更新的 Shader Model 可以支持更长的程序、更强大的分支指令和其他功能，并且这些特性已经让游戏的图形性能得到改善。

Unity Editor 可以对多组 Shader Model 和图形 API 限制进行仿真，以便快速了解在特定 GPU 或图形 API 上运行时游戏的外观概况。请注意，虽然Editor 内的仿真非常近似，但是仍然建议在目标硬件上实际运行游戏程序。


要选择图形仿真级别，请选择 __Edit__ > __Graphics Emulation__ 菜单。请注意，可用选项会根据您在 [Build Settings](BuildSettings.html) 中当前设置的目标平台而不同。您可以通过选择 __No Emulation__ 来恢复硬件的完整功能。如果您的开发计算机不支持特定 Shader Model，则该菜单项将被禁用。




#### Shader Model 4（独立平台和通用 Windows 平台）


* 对 DirectX 10 功能集仿真（2007 到 2009 年期间制造的 PC GPU）。
* 关闭对[计算着色器](ComputeShaders.html)和相关功能（计算缓冲区、随机写入纹理）、[稀疏纹理](SparseTextures.html)和曲面细分着色器的支持。




#### Shader Model 3（独立平台）


* 对 DirectX 9 SM3.0 功能集仿真（2004 到 2006 年期间制造的 PC GPU）。
* 除了由 Shader Model 4 仿真关闭的功能外，这还会关闭对[绘制调用实例化](GPUInstancing.html)、[纹理数组](SL-TextureArrays.html)和几何着色器的支持。它最多可以强制执行 4 个同步渲染目标，并且在单个着色器中最多使用 16 个纹理。




#### Shader Model 2（独立平台）


* 对 DirectX 9 SM2.0 功能集仿真（2002 到 2004 年期间制造的 PC GPU）。
* 除了由 Shader Model 3 仿真关闭的功能外，这还会关闭对 [HDR 渲染](HDR.html)、[线性颜色空间](LinearLighting.html)和[深度纹理](SL-DepthTextures.html)的支持。




#### OpenGL ES 3.0（Android 平台）


* 对移动端 OpenGL ES 3.0 功能集仿真。
* 关闭对[计算着色器](ComputeShaders.html)和相关功能（计算缓冲区、随机写入纹理）、[稀疏纹理](SparseTextures.html)、曲面细分着色器和几何着色器的支持。最多可以强制执行 4 个同步渲染目标，并且在单个着色器中最多使用 16 个纹理。允许的最大纹理大小设置为 4096，最大立方体贴图大小设置为 2048。禁用实时柔和阴影。




#### Metal（iOS 和 tvOS 平台）


* 对移动端 Metal 功能集仿真。
* 适用的限制与 GLES3.0 仿真相同，只是最大立方体贴图大小设置为 4096。




#### OpenGL ES 2.0（Android、iOS 和 tvOS 平台）


* 对移动端 OpenGL ES 2.0 功能集仿真。
* 除了由 GLES3.0 仿真关闭的功能外，这还会关闭对[绘制调用实例化](GPUInstancing.html)、[纹理数组](SL-TextureArrays.html)、3D 纹理和多渲染目标的支持。强制在单个着色器中最多使用 8 个纹理。允许的最大立方体贴图大小设置为 1024。


#### WebGL 1 和 WebGL 2（WebGL 平台）


* 对典型 [WebGL](webgl-graphics.html) 图形限制进行仿真。
* 与上述 GLES2.0 和 GLES3.0 仿真级别非常类似，只是支持的纹理大小更高（对于常规纹理，大小为 8192，对于立方体贴图，大小为 4096），而且在单个着色器中允许 16 个纹理。


#### Shader Model 2 - DX11 FL9.3（通用 Windows 平台）


* 对典型 Windows Phone 图形功能集仿真。
* 与 Shader Model 2 仿真非常类似，但还禁用了多目标渲染和单独 Alpha 混合。

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
