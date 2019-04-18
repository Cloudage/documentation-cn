# 使用 Cg/HLSL 访问着色器属性


着色器在 [Properties](SL-Properties.html) 代码块中声明材质属性。如果要在[着色器程序](SL-ShaderPrograms.html)中访问其中一些属性，则需要声明具有相同名称和匹配类型的 Cg/HLSL 变量。[着色器教程：顶点和片元程序](ShaderTut2.html)中提供了一个示例。

例如，以下着色器属性：


````
_MyColor ("Some Color", Color) = (1,1,1,1) 
_MyVector ("Some Vector", Vector) = (0,0,0,0) 
_MyFloat ("My float", Float) = 0.5 
_MyTexture ("Texture", 2D) = "white" {} 
_MyCubemap ("Cubemap", CUBE) = "" {}
````

可通过如下 Cg/HLSL 代码进行声明以供访问：


````
fixed4 _MyColor; // 低精度类型对于颜色而言通常已经足够
float4 _MyVector;
float _MyFloat; 
sampler2D _MyTexture;
samplerCUBE _MyCubemap;
````

Cg/HLSL 还可以接受 __uniform__ 关键字，但该关键字并不是必需的：


````
uniform float4 _MyColor;
````

ShaderLab 中的属性类型以如下方式映射到 Cg/HLSL 变量类型：

* Color 和 Vector 属性映射到 __float4__、__half4__ 或 __fixed4__ 变量。
* Range 和 Float 属性映射到 __float__、__half__ 或 __fixed__ 变量。
* 对于普通 (2D) 纹理，Texture 属性映射到 __sampler2D__ 变量；立方体贴图 (Cubemap) 映射到 __samplerCUBE__；3D 纹理映射到 __sampler3D__。


## 如何向着色器提供属性值

在下列位置中查找着色器属性值并提供给着色器：

* [MaterialPropertyBlock](../ScriptReference/MaterialPropertyBlock.html) 中设置的每渲染器值。这通常是“每实例”数据（例如，全部共享相同材质的许多对象的自定义着色颜色）。
* 在渲染的对象上使用的[材质](class-Material.html)中设置的值。
* 全局着色器属性，通过 Unity 渲染代码自身设置（请参阅[内置着色器变量](SL-UnityShaderVariables.html)），或通过您自己的脚本来设置（例如 [Shader.SetGlobalTexture](../ScriptReference/Shader.SetGlobalTexture.html)）。

优先顺序如上所述：每实例数据覆盖所有内容；然后使用材质数据；最后，如果这两个地方不存在着色器属性，则使用全局属性值。最终，如果在任何地方都没有定义着色器属性值，则将提供“默认值”（浮点数的默认值为零，颜色的默认值为黑色，纹理的默认值为空的白色纹理）。


## 序列化和运行时材质属性

[材质](class-Material.html)可以同时包含序列化的属性值和运行时设置的属性值。

序列化的数据是在着色器的 [Properties](SL-Properties.html) 代码块中定义的所有属性。通常，这些是需要存储在材质中的值，并且可由用户在材质检视面板中进行调整。

材质也可以具有着色器使用的一些属性，但不在着色器的 [Properties](SL-Properties.html) 代码块中声明。通常，这适用于在运行时从脚本代码（例如，通过 [Material.SetColor](../ScriptReference/Material.SetColor.html)）设置的属性。请注意，矩阵和数组只能作为非序列化的运行时属性存在（因为无法在 Properties 代码块中定义它们）。


## 特殊纹理属性

对于设置为着色器/材质属性的每个纹理，Unity 还会在其他矢量属性中设置一些额外信息。


#### 纹理平铺和偏移

[材质](class-Material.html)通常具有其纹理属性的 Tiling 和 Offset 字段。此信息将传递到着色器中的 float4 _{TextureName}_`_ST` 属性：

* `x` 包含 X 平铺值
* `y` 包含 Y 平铺值
* `z` 包含 X 偏移值
* `w` 包含 Y 偏移值

例如，如果着色器包含名为 `_MainTex` 的纹理，则平铺信息将位于 `_MainTex_ST` 矢量中。


#### 纹理大小

_{TextureName}_`_TexelSize` - float4 属性包含纹理大小信息：

* `x` 包含 1.0/宽度
* `y` 包含 1.0/高度
* `z` 包含宽度
* `w` 包含高度


#### 纹理 HDR 参数

_{TextureName}_`_HDR` - 一个 float4 属性，其中包含有关如何根据所使用的[颜色空间](LinearLighting.html)解码潜在 HDR（例如 RGBM 编码）纹理的信息。请参阅 [UnityCG.cginc](SL-BuiltinIncludes.html) 着色器 include 文件中的 `DecodeHDR` 函数。


## 颜色空间和颜色/矢量着色器数据

使用[线性颜色空间](LinearLighting.html)时，所有材质颜色属性均以 sRGB 颜色提供，但在传递到着色器时会转换为线性值。

例如，如果 [Properties](SL-Properties.html) 着色器代码块包含名为“_MyColor”的 `Color` 属性，则相应的“_MyColor”HLSL 变量将获得线性颜色值。

对于标记为 `Float` 或 `Vector` 类型的属性，默认情况下不进行颜色空间转换；而是假设它们包含非颜色数据。可为浮点/矢量属性添加 `[Gamma]` 特性，以表示它们是以 sRGB 空间指定，就像颜色一样（请参阅[属性](SL-Properties.html)）。


## 另请参阅

* [ShaderLab Properties 代码块](SL-Properties.html)。
* [编写着色器程序](SL-ShaderPrograms.html)。
