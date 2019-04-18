#升级到 Unity 5.5




### 着色器：基于物理着色代码更改

基于物理着色的相关代码在 Unity 5.5 中已重构
（文件 `UnityStandardBRDF.cginc` 等）。在大多数
情况下，这不会直接影响着色器代码，除非
手动直接调用某些函数。下面
列出了一些值得注意的更改。

- 现在可以使用函数在平滑度、粗糙度和感知粗糙度之间
进行转换：
`PerceptualRoughnessToRoughness`、
`RoughnessToPerceptualRoughness`、`SmoothnessToRoughness`、
`RoughnessToSmoothness`。

- `UnityStandardBRDF.cginc` 中的可见性术语采用 `roughness` 而不是 `perceptualRoughness`。

- 在旧版本的 Unity 中，可使用 Marmoset 粗糙度进行重新映射。转移到 GGX 后便不再匹配，并且已删除 `UNITY_GLOSS_MATCHES_MARMOSET_TOOLBAG2` 定义和相关代码。

- 对 Gbuffer 的所有读写操作都应该通过新函数 `UnityStandardDataToGbuffer` / `UnityStandardDataFromGbuffer` 进行。

- 着色器代码应调用 `UnityGlossyEnvironmentSetup()` 来初始化 `Unity_GlossyEnvironmentData` 结构而不是手动执行初始化。

- `Unity_GlossyEnvironmentData` 的 `roughness` 变量实际上是“感知粗糙度”，但它尚未经过重命名来避免现有着色器代码的错误。注意：`UnityGlossyEnvironmentSetup` 采用 `smoothness` 作为参数并计算感知粗糙度。

- 现在可以动态计算 `UnityLight` 中的 `ndotl` 变量值，并会忽略写入该变量的任何值。

- `UNITY_GI` 宏已弃用，不应再使用。


###着色器：DirectX 9 半像素偏移问题

Unity 5.5 现在在后台处理 DX9 半像素偏移光栅化，这意味着您不再需要在着色器或代码中修复 DX9 半像素问题。请参阅[此博客文章](http://aras-p.info/blog/2016/04/08/solving-dx9-half-pixel-offset/)了解更多详细信息。如果在代码中使用了以下任何检查，现在可以删除它们：

- 检查着色器中的 UNITY_HALF_TEXEL_OFFSET，并据此移动顶点/UV 的问题。
- 通过 SystemInfo.graphicsDeviceType 或 SystemInfo.graphicsDeviceVersion 检查 D3D9，并据此移动顶点/UV。

Unity 现在解决这个问题的方法是将半像素调整代码插入到正在编译的所有顶点着色器中。结果，Unity 会保留顶点着色器常量寄存器 c255，还会向所有着色器添加两条指令，并使用一个额外的临时寄存器。这样应该不会产生问题，除非顶点着色器将所有可用资源（常量/临时寄存器和指令字段）用尽至绝对最大值。


###着色器：Z 缓冲区浮点数反转

Z 缓冲区（深度缓冲区）方向已被反转，这意味着现在 Z 缓冲区在近平面处包含1.0，而在远平面处包含 0.0。这种设置与浮点深度缓冲区相结合可显著提高深度缓冲精度，从而减少 Z 冲突并改善阴影，尤其是在使用小的近平面和大的远平面时。


图形 API 更改：

- 裁剪空间范围是 [near, 0] 而不是 [0, far]
- _CameraDepthTexture 纹理范围是 [1,0] 而不是 [0,1]
- 在应用 Z-bias 之前对其求反
- 24 位深度缓冲区切换为 32 位浮点格式


以下宏/函数将处理反转 Z 的情况，无需任何其他步骤。如果您的着色器已经在使用它们，那么您无需进行任何更改：

- Linear01Depth(float z)
- LinearEyeDepth(float z)
- UNITY_CALC_FOG_FACTOR(coord)

但是，如果要手动获取 Z 缓冲区值，则需要执行如下所示的编写代码：

```
float z = tex2D(_CameraDepthTexture, uv);
# if defined(UNITY_REVERSED_Z)
    z = 1.0f - z;
# endif
```

对于裁剪空间深度，可以使用以下宏。请**注意**这个宏不会改变 OpenGL/ES 平台上的裁剪空间，但会保持 `[-near, far]`：

```
float clipSpaceRange01 = UNITY_Z_0_FAR_FROM_CLIPSPACE(rawClipSpace);
```

现在，**_ZBufferParams** 在具有反转深度缓冲区的平台上包含以下值。请参阅有关[平台特定的渲染差异](SL-PlatformDifferences.html)的文档以了解更多信息。

```
x = -1+far/near
y = 1
z = x/far
w = 1/far
```

Z-bias 由 Unity 自动处理，但是，如果要使用本机代码渲染插件，则需要针对匹配的平台在 C/C++ 代码中反转该偏差。


## 特殊文件夹：名称为“Resources”的 Unity Editor 子文件夹


名为__“Editor”__的文件夹的所有子文件夹都将排除在构建之外，并且不会在 Unity Editor 中以播放模式加载。以前一个名为__“Resources”__的子文件夹会将其资源包含在构建中。通过在 Editor 脚本中调用 Resources.Load()，仍可加载这些资源。

例如，以下文件会排除在构建之外，并在 Editor 中处于播放模式时__不会__加载，但如果从脚本进行调用则会__加载__：

- Editor/Foo/Resources/Bar.png*（从 Editor 代码中加载为“Bar.png”）*
- Editor/Resources/Foo.png
- Editor/Resources/Editor/Resources/Foo.png*（从 Editor 代码中加载为“Foo.png”而不是加载为“Editor/Resources/Foo.png”）*

以下资源将在所有情况下加载：

- Resources/Editor/Foo.png
- Resources/Foo/Editor/Bar.png*（加载为“Foo/Editor/Bar.png”）*
- Resources/Editor/Resources/Foo.png*（加载为“Foo.png”而不是加载为“Editor/Resources/Foo.png”）*


## Backface Tolerance（背面公差）和 Final Gather（最终收集）

Previously the ‘Backface Tolerance’ parameter in [Lightmap Parameters](class-LightmapParameters.html) was not applied when using final gather for baked GI. It is now applied correctly. The parameter now affects both the realtime GI and baked GI stages (including the final gather).

受影响的场景主要是具有单面几何体（如公告牌）的场景，这种情况下需要调整“Backface Tolerance”以避免使看到单面几何体背面的纹素无效。在使用公告牌和最终收集的场景中，现在可以通过调整“Backface Tolerance”来改善光照贴图，但是如果应用非默认的“Backface Tolerance”，其他场景也可能会受到影响，因为现在在最终收集阶段可对其进行正确计算。


## 标准着色器 BRDF2 现在使用 GGX 近似算法

BRDF2 是在移动平台上默认设置的标准着色器类型，现在使用 GGX 近似算法而不是 Blinn-Phong。这使其看起来更接近 BRDF1（默认情况下在桌面平台上使用）并可改善视觉质量。

如果需要保留旧版近似算法，则应修改 UnityStandardBRDF.cginc 中的 BRDF2 代码，它在 #if UNITY_BRDF_GGX 语句中具有新实现（这也由 BRDF1 用来选取 GGX）。请更改 UnityStandardConfig.cginc 中的定义，或将 #if UNITY_BRDF_GGX 更改为 #if 0 in the BRDF2_Unity_PBS 函数。

## Gradle for Android

现在可以使用 [Gradle](android-gradle-overview.html) 进行以 Android 为目标的构建。

与现有的 Unity Android 构建系统相比，Gradle 对错误并不严格，这意味着一些现有项目可能很难转换为 Gradle。请参阅有关 [Gradle 故障排除](android-gradle-troubleshooting.html)的文档来识别并解决这些构建故障。


## 实例化对象重载已更改

Instantiate 函数的特定重载在默认情况下为原始游戏对象采用一个参数并为父变换组件采用一个参数，现在已更改为以不同方式工作。它不再将原始游戏对象的位置和旋转解释为世界空间位置和旋转，因此会忽略指定的父变换组件的位置和旋转。

现在，它默认将位置和旋转解释为指定父变换组件空间内的局部位置和旋转。这与 Editor 中的行为一致。您的脚本不会自动更新。这意味着，如果运行的脚本包含对此 Instantiate 重载的调用但尚未更新来体现此更改，则可能会遇到意外行为。


##渲染器和 LOD 组 (LOD Group) 组件的行为

禁用 LOD 组 (LOD Group) 组件不再禁用附加到该组件上的渲染器。LOD 组设置仅在启用 LOD 组组件时才适用于渲染器。升级项目时，Unity 会自动应用此更改，并且无法还原更改。
