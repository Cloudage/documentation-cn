# 着色器编译目标级别

编写[表面着色器](SL-SurfaceShaders.html)或常规
[着色器程序](SL-ShaderPrograms.html)时，HLSL 源代码可以
编译到不同“着色器模型”中。
为了支持使用更现代的 GPI 功能，必须使用更高的着色器编译目标。

**注意**：使用更高的着色器编译目标可能会阻止着色器在较旧的 GPU 或平台上运行。

应使用 `#pragma target` *名称* 指令来支持编译目标。例如：


```
# pragma target 3.5
```

## 默认编译目标

默认情况下，Unity 将着色器编译为几乎支持的最低目标（“2.5”）；处于 DirectX 着色器模型 2.0 和 3.0 之间。其他一些编译指令使着色器自动
编译成更高的目标：

* 使用几何着色器 (`#pragma geometry`) 将编译目标设置为 `4.0`。
* 使用曲面细分着色器（`#pragma hull` 或 `#pragma domain`）将编译目标设置为 `4.6`。

对于几何体、外壳或域着色器，未通过 `#pragma` 显式设置函数入口点的任何着色器都将降级内部着色器功能要求。这可使具有更广泛运行时和功能差异的非 DX11 目标与现有着色器内容更加兼容。

例如，Unity 在 Metal 图形上支持曲面细分着色器，但 Metal 不支持几何着色器。使用 `#pragma target 5.0` 仍有效，只要您不使用几何着色器。


## 支持的 #pragma 目标名称

以下是支持的着色器模型列表，其中包含大致增加的功能集（在某些情况下对于平台/GPU 的要求更高）：

#### #pragma target 2.0

*适用于 Unity 支持的所有平台。DX9 着色器模型 2.0。
* 有限数量的算术和纹理指令；8 个插值器；没有顶点纹理采样；片元着色器中没有衍生指令；没有显式的 LOD 纹理采样。

#### #pragma target 2.5（默认值）

* 几乎与 3.0 目标相同（见下文），例外之处是仍然只有
 8 个插值器，并且没有显式的 LOD 纹理采样。
* 在 Windows Phone 上编译为 DX11 功能级别 9.3。

#### #pragma target 3.0

* DX9 着色器模型 3.0：衍生指令，纹理 LOD 采样，10 个插值器，允许更多的数学/纹理指令。
* 在 DX11 功能级别 9.x GPU（例如大多数 Windows Phone 设备）上不支持。
* 某些 OpenGL ES 2.0 设备可能无法完全支持，具体取决于存在的驱动程序扩展和使用的功能。

#### #pragma target 3.5（或 es3.0）

* OpenGL ES 3.0 功能（D3D 平台上的 DX10 SM4.0，只是没有几何着色器）。
* 在 DX11 9.x (WinPhone) 和 OpenGL ES 2.0 上不支持。
* 在 DX11+、OpenGL 3.2+、OpenGL ES 3+、Metal、Vulkan 和 PS4/XB1 游戏主机上支持。
* 着色器、纹理数组等中的本机整数运算。

#### #pragma target 4.0

* DX11 着色器模型 4.0。
* 在 DX11 9.x (WinPhone)、OpenGL ES 2.0/3.0/3.1 和 Metal 上不支持。
* 在 DX11+、OpenGL 3.2+、OpenGL ES 3.1+AEP、Vulkan 和 PS4/XB1 游戏主机上支持。
* 具有几何着色器以及 `es3.0` 目标所具有的一切功能。

#### #pragma target 4.5（或 es3.1）

* OpenGL ES 3.1 功能（D3D 平台上的 DX11 SM5.0，只是没有曲面细分着色器）。
* 在早于 SM5.0 的 DX11、早于 4.3 的 OpenGL（即 Mac）和 OpenGL ES 2.0/3.0 上不支持。
* 在 DX11+ SM5.0、OpenGL 4.3+、OpenGL ES 3.1、Metal、Vulkan 和 PS4/XB1 游戏主机上支持。
* 有计算着色器、随机访问纹理写入、原子等。没有几何着色器和曲面细分着色器。

#### #pragma target 4.6（或 gl4.1）

* OpenGL 4.1 功能（D3D 平台上的 DX11 SM5.0，只是没有计算着色器）。这基本上是 Mac 支持的最高
 OpenGL 级别。
* 在早于 SM5.0 的 DX11、早于 4.1 的 OpenGL、OpenGL ES 2.0/3.0/3.1 和 Metal 上不支持。
* 在 DX11+ SM5.0、OpenGL 4.1+、OpenGL ES 3.1+AEP、Vulkan、Metal（不含几何体）和 PS4/XB1 游戏主机上支持。

#### #pragma target 5.0

* DX11 着色器模型 5.0。
* 在早于 SM5.0 的 DX11、早于 4.3 的 OpenGL（即 Mac）、OpenGL ES 2.0/3.0/3.1 和 Metal 上不支持。
* 在 DX11+ SM5.0、OpenGL 4.3+、OpenGL ES 3.1+AEP、Vulkan、Metal（不含几何体）和 PS4/XB1 游戏主机上支持。



请注意，所有 OpenGL 类平台（包括移动平台）都被视为“支持着色器模型 3.0”。WP8/WinRT 平台（DX11 功能级别 9.x）被视为仅支持着色器模型 2.5。


## 另请参阅

* [编写着色器程序](SL-ShaderPrograms.html)。
* [表面着色器](SL-SurfaceShaders.html)。

---

* <span class="page-edit">2018-03-20  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>
* <span class="page-history">在 Unity [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 中添加了着色器 #pragma 指令 <span class="search-words">NewIn20181</span></span>
* <span class="page-history">在 2018.1 版中添加了__Metal 曲面细分__</span>

