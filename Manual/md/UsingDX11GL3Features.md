# DirectX

To set DirectX11 as your default Graphics API in the Editor or Standalone Player, go to the Player Settings (menu: __Edit &gt; Project Settings &gt; Player__) and navigate to __Other Settings__. Uncheck __Auto Graphics API for Windows__, and choose __DirectX11__ from the list. For more details, see [Graphics API support](Graphics APIs).

## Surface Shaders

Some parts of the [Surface Shader](SL-SurfaceShaders.html) compilation pipeline do not understand DX11-specific HLSL syntax, so if you’re using HLSL features like StructuredBuffers, RWTextures and other non-DX9 syntax, you need to wrap it into a DX11-only preprocessor macro. See documentation on [Platform-specific differences](SL-PlatformDifferences.html) for more information.

## 曲面细分和几何着色器

表面着色器支持简单的曲面细分和移位。请参阅有关[表面着色器曲面细分](SL-SurfaceShaderTessellation.html)的文档以了解更多信息。

When manually writing [Shader programs](SL-ShaderPrograms.html), you can use the full set of DX11 Shader model 5.0 features, including Geometry, Hull and Domain Shaders.

Tessellation and geometry shaders are only supported by a subset of graphics APIs. This is controlled by the [Shader Compilation Target level](SL-ShaderCompileTargets.html).

## 计算着色器

Compute Shaders run on the graphics card and can speed up rendering. See documentation on [Compute Shaders](ComputeShaders.html) for more information.

---

* <span class="page-edit">2018-06-02  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>

