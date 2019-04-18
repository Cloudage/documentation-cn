# 表面着色器中的自定义光照模型

编写[表面着色器](SL-SurfaceShaders.html)时，您将描述表面的属性（比如反照率颜色和法线），而由__光照模型__计算光照交互。

有两个内置光照模型，分别是 `Lambert` 和 `BlinnPhong`，前者用于漫射光照，后者用于镜面反射光照。Unity 中的 _Lighting.cginc_ 文件用于定义这些模型（Windows：_&lt;unity 安装路径&gt;/Data/CGIncludes/Lighting.cginc_；macOS：_/Applications/Unity/Unity.app/Contents/CGIncludes/Lighting.cginc_）。

有些时候，您可能希望使用自定义光照模型。为此，可使用表面着色器。光照模型实际上就是若干符合某些惯例的 Cg/HLSL 函数。


## 声明光照模型

一个光照模型中包含多个名称以 `Lighting` 开头的常规函数。您可以在着色器文件中的任何位置声明这些函数，也可以在其中一个包含的文件中声明。这些函数是：

1.`half4 Lighting<Name> (SurfaceOutput s, UnityGI gi);`
  在_不依赖于_视图方向的光照模型的前向渲染路径中使用此函数。

1.`half4 Lighting<Name> (SurfaceOutput s, half3 viewDir, UnityGI gi);`
  在_依赖于_视图方向的光照模型的前向渲染路径中使用此函数。

1.`half4 Lighting<Name>_Deferred (SurfaceOutput s, UnityGI gi, out half4 outDiffuseOcclusion, out half4 outSpecSmoothness, out half4 outNormal);`
  在延迟光照路径中使用此函数。

1.`half4 Lighting<Name>_PrePass (SurfaceOutput s, half4 light);`
  在光照预通道（旧版延迟）光照路径中使用此函数。

请注意，您无需声明所有函数。光照模型不一定会使用视图方向。同样，如果仅在前向渲染中使用光照模型，请勿声明 `_Deferred` 和 `_Prepass` 函数。这确保了使用视图方向的着色器仅编译到前向渲染。

## 自定义 GI

声明以下函数可自定义光照贴图数据和探针的解码：

1.`half4 Lighting<Name>_GI (SurfaceOutput s, UnityGIInput data, inout UnityGI gi);`

请注意，要对 Unity 标准光照贴图和 SH 探针进行解码，您可以使用内置函数 `DecodeLightmap` 和 `ShadeSHPerPixel`；这些函数位于 Unity 内部 _UnityGlobalIllumination.cginc_ 文件中的 `UnityGI_Base` 内（Windows：_&lt;unity 安装路径&gt;/Data/CGIncludes/UnityGlobalIllumination.cginc_；macOS：_/Applications/Unity/Unity.app/Contents/CGIncludes/UnityGlobalIllumination.cginc__）。

## 示例
请参阅有关[表面着色器光照示例](SL-SurfaceShaderLightingExamples.html)的文档以了解更多信息。
