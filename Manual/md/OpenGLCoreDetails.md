#OpenGL Core

OpenGL Core is a back-end capable of supporting the latest OpenGL features on Windows, MacOS X and Linux. This scales from OpenGL 3.2 to OpenGL 4.5, depending on the OpenGL driver support.

## 启用 OpenGL Core

To set OpenGL Core as your default Graphics API in the Editor or Standalone Player, go to the Player Settings (menu: __Edit &gt; Project Settings &gt; Player__), and navigate to __Other Settings__. Uncheck __Auto Graphics API for Windows__, and choose __OpenGLCore__ from the list. For more details, see [Graphics API support](Graphics APIs).

## OpenGL requirements

OpenGL Core 的最低要求如下：

* Mac OS X 10.8 (OpenGL 3.2)、MacOSX 10.9（OpenGL 3.2 到 4.1）

* Windows 搭载 NVIDIA 最低 2006 (GeForce 8)、AMD 最低 2006 (Radeon HD 2000)、Intel 最低 2012 (HD 4000 / IvyBridge)（OpenGL 3.2 到 OpenGL 4.5）

* Linux（OpenGL 3.2 到 OpenGL 4.5）

## macOS OpenGL driver limitations

The macOS OpenGL backend for the Editor and Standalone supports OpenGL 3.x and 4.x features such as tessellation and geometry shaders. 

但是，由于 Apple 将 OS X 桌面上的 OpenGL 版本限制为最高 4.1，因此并不支持所有 DirectX 11 功能（例如无序访问视图或计算着色器）。这意味着，以着色器级别 5.0 为目标（包含 #pragma 目标 50）的所有着色器将无法在 OS X 上加载。

因此引入了新的着色器目标级别：#pragma 目标 gl4.1。此目标级别至少需要 OpenGL 4.1 或 DirectX 11.0 着色器级别 5（桌面端）或 OpenGL ES 3.1 + Android 扩展包（移动端）。

##OpenGL Core 功能

新的 OpenGL 后端引入了许多新功能（以前主要是 DX11/GLES3）：

- 计算着色器（以及 ComputeBuffer 和“随机写入”渲染纹理）
- 曲面细分和几何着色器
- 间接绘制（Graphics.DrawProcedural 和 Graphics.DrawProceduralIndirect）
- 高级混合模式


##着色器更改

使用现有 <span class="docs-keyword">#pragma</span> 目标时，它们会映射到以下 GL 级别：

- \#pragma 目标 4.0 // OpenGL ES 3.1、桌面版 OpenGL 3.x、DX Shader Model 4.0
- \#pragma 目标 gl4.1 // 桌面版 OpenGL 4.1、SM 4.0 + 曲面细分（匹配 MacOSX 10.9 功能）
- \#pragma 目标 5.0 // OpenGL ES 3.1 + Android 扩展包、桌面版 OpenGL >= 4.2、DX Shader Model 5.0

要让着色器平台使用或不使用特定着色器，可使用以下 <span class="docs-keyword">#pragma</span> only_renderers / exclude_renderers 目标：

- **#pragma only_renderers glcore**：仅针对桌面版 GL 进行编译。与 ES 3 目标一样，这也可扩展到包含所有桌面 GL 版本，这些版本中的基本着色器将支持 GL 2.x，而需要 SM5.0 功能的着色器需要 OpenGL 4.2+。


##OpenGL Core 配置文件命令行参数

可使用命令行参数通过 OpenGL 启动 Editor 或播放器：

- **-force-opengl**：使用旧版 OpenGL 后端
- **-force-glcore**：使用新版 OpenGL 后端。通过使用此参数，Unity 将检测平台支持的所有功能，从而在运行时使用可能最好的 OpenGL 版本和所有可用的 OpenGL 扩展
- **-force-glcoreXY**：XY 可以是 32、33、40、41、42、43、44 或 45；每个数字代表 OpenGL 的特定版本。如果平台不支持 OpenGL 的特定版本，Unity 将回退到支持的版本
- **-force-clamped**：请求 Unity 不使用 OpenGL 扩展，这样可确保多个平台将执行相同的代码路径。这是一种测试某个问题是否特定于平台（例如驱动程序错误）的方法。

##原生 OpenGL ES 桌面版命令行参数

OpenGL ES 图形 API 适用于配备了 Intel 或 NVIDIA GPU 且驱动程序支持 OpenGL ES 的 Windows 机器。

- **-force-gles**：在 OpenGL ES 模式下使用新版 OpenGL 后端。通过使用此参数，Unity 将检测平台支持的所有功能，从而在运行时使用可能最好的 OpenGL ES 版本和所有可用的 OpenGL ES 扩展
- **-force-glesXY**：XY 可以是 20、30、31 或 31aep；每个数字代表 OpenGL ES 的特定版本。如果平台不支持 OpenGL ES 的特定版本，Unity 将回退到支持的版本。如果平台不支持 OpenGL ES，Unity 将回退到另一个图形 API。
- **-force-clamped**：请求 Unity 不使用 OpenGL 扩展，这样可确保多个平台将执行相同的代码路径。这是一种测试某个问题是否特定于平台（例如驱动程序错误）的方法。

---

* <span class="page-edit">2018-06-02  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>

