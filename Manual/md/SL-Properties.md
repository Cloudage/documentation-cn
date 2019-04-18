#ShaderLab：Properties

着色器可以定义 Unity [材质检视面板](Materials.html)中由美术师设置的参数列表。[着色器文件](SL-Shader.html)中的 Properties 代码块将定义这些属性。

##语法


#### Properties
````
Properties { Property [Property ...]}
````
定义属性代码块。在大括号内，多个属性定义如下。


#### 数字和滑动条
````
name ("display name", Range (min, max)) = number
name ("display name", Float) = number
name ("display name", Int) = number
````
这些全都定义一个具有默认值的数字（标量）属性。`Range` 格式使该属性显示为一个滑动条，范围在 *min* 到 *max* 之间。


#### 颜色和矢量
````
name ("display name", Color) = (number,number,number,number)
name ("display name", Vector) = (number,number,number,number)
````
使用给定 RGBA 分量的默认值定义颜色属性，或使用默认值定义 4D 矢量属性。颜色属性会显示拾色器，并根据颜色空间按需进行调整（请参阅[着色器程序中的属性](SL-PropertiesInPrograms.html)）。矢量属性显示为四个数字字段。


#### 纹理
````
name ("display name", 2D) = "defaulttexture" {}
name ("display name", Cube) = "defaulttexture" {}
name ("display name", 3D) = "defaulttexture" {}
````
分别定义 [2D 纹理](class-TextureImporter.html)、[立方体贴图](class-Cubemap.html)或 [3D（体积）](class-Texture3D.html)属性。



## 详细信息

着色器中的每个属性均通过 **name** 引用（在 Unity 中，着色器属性名称通常以下划线开头）。属性在材质检视面板中将显示为 **display name**。每个属性都在等号后给出默认值：

* 对于 _Range_ 和 _Float_ 属性，默认值仅仅是单个数字，例如“13.37”。
* 对于 _Color_ 和 _Vector_ 属性，默认值是括在圆括号中的四个数字，例如“(1,0.5,0.2,1)”。
* 对于 2D 纹理，默认值为空字符串或内置默认纹理之一：“white”（RGBA：1,1,1,1）、“black”（RGBA：0,0,0,0）、“gray”（RGBA：0.5,0.5,0.5,0.5）、“bump”（RGBA：0.5,0.5,1,0.5）或“red”（RGBA：1,0,0,0）。
* 对于非 2D 纹理（立方体、3D 或 2D 数组），默认值为空字符串。如果材质未指定立方体贴图/3D/数组纹理，则使用灰色（RGBA：0.5,0.5,0.5,0.5）。

稍后在着色器的固定函数部分中，可使用括在方括号中的属性名称来访问属性值：**[name]**。例如，可通过声明两个整数属性（例如“_SrcBlend”和“_DstBlend”）来使混合模式由材质属性驱动，然后让 [Blend 命令](SL-Blend.html)使用它们：`Blend [_SrcBlend] [_DstBlend]`。

`Properties` 代码块中的着色器参数被序列化为[材质](Materials.html)数据。[着色器程序](SL-ShaderPrograms.html)实际上可以有更多参数（如矩阵、矢量和浮点数），这些参数在运行时从代码中在材质上设置，但如果它们不是 Properties 代码块的一部分，则不会保存它们的值。这对于完全由脚本代码驱动的值最有用（使用 [Material.SetFloat](../ScriptReference/Material.SetFloat.html) 和类似函数）。


### 属性特性和绘制器

在属性前面，可指定可选的特性（用方括号括起）。这些是 Unity 可以识别的特性，或者它们可以指示您自己的 [MaterialPropertyDrawer 类](../ScriptReference/MaterialPropertyDrawer.html) 来控制它们在[材质检视面板](class-Material.html)中的呈现方式。Unity 可以识别的特性包括：

* `[HideInInspector]` - 不在材质检视面板中显示属性值。
* `[NoScaleOffset]` - 对于具有此特性的纹理属性，材质检视面板不会显示纹理平铺/偏移字段。
* `[Normal]` - 表示纹理属性需要法线贴图。
* `[HDR]` - 表示纹理属性需要高动态范围 (HDR) 纹理。
* `[Gamma]` - 表示在 UI 中将浮点/矢量属性指定为 sRGB 值（就像颜色一样），并且可能需要根据使用的颜色空间进行转换。请参阅[着色器程序中的属性](SL-PropertiesInPrograms.html)。
* `[PerRendererData]` - 表示纹理属性将以 [MaterialPropertyBlock](../ScriptReference/MaterialPropertyBlock.html) 的形式来自每渲染器数据。材质检视面板会更改这些属性的纹理字段 UI。


## 示例

````
// 水着色器的属性
Properties
{
    _WaveScale ("Wave scale", Range (0.02,0.15)) = 0.07 // 滑动条
    _ReflDistort ("Reflection distort", Range (0,1.5)) = 0.5
    _RefrDistort ("Refraction distort", Range (0,1.5)) = 0.4
    _RefrColor ("Refraction color", Color) = (.34, .85, .92, 1) // 颜色
    _ReflectionTex ("Environment Reflection", 2D) = "" {} // 纹理
    _RefractionTex ("Environment Refraction", 2D) = "" {}
    _Fresnel ("Fresnel (A) ", 2D) = "" {}
    _BumpMap ("Bumpmap (RGB) ", 2D) = "" {}
}
````


## 纹理属性选项（5.0 中已删除）

在 Unity 5 之前，纹理属性可以将选项括在花括号代码块中，例如 `TexGen CubeReflect`。这些纹理属性用于控制固定函数纹理坐标的生成。5.0 中删除了此功能；如果现在需要 texgen，应编写一个[顶点着色器](SL-ShaderPrograms.html)来代替。请参阅[实现固定函数 TexGen](SL-ImplementingTexGen.html)页面以查看示例。


## 另请参阅

* [使用 HLSL/Cg 访问着色器属性](SL-PropertiesInPrograms.html)。
* [材质属性绘制器](../ScriptReference/MaterialPropertyDrawer.html)。
