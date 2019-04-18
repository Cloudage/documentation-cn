#ShaderLab：Pass

Pass 代码块将使游戏对象的几何体被渲染一次。


##语法

````
Pass { [Name and Tags] [RenderSetup] }
````

基本的 Pass 命令包含渲染状态设置命令的列表。


##名称和标签

一个通道 (Pass) 可以定义一个[名称 (Name)](SL-Name.html) 和任意数量的[标签 (Tags)](SL-PassTags.html)。这些名称/值字符串用于将通道的意图传达给渲染引擎。

##渲染状态设置

一个通道将设置图形硬件的各种状态，例如是否应开启 Alpha 混合或是否应使用深度测试。

命令如下所示：


###Cull
````
Cull Back | Front | Off
````
设置多边形剔除模式。


###ZTest
````
ZTest (Less | Greater | LEqual | GEqual | Equal | NotEqual | Always)
````
设置深度缓冲区测试模式。


###ZWrite
````
ZWrite On | Off
````
设置深度缓冲区写入模式。

###Offset
````
Offset OffsetFactor, OffsetUnits
````
设置 Z 缓冲区深度偏移。请参阅[剔除和深度页面](SL-CullAndDepth.html)的文档以了解更多详细信息。

请参阅[剔除和深度](SL-CullAndDepth.html)的文档以了解有关 Cull、ZTest、ZWrite 和 Offset 的更多详细信息。

###Blend
````
Blend sourceBlendMode destBlendMode
Blend sourceBlendMode destBlendMode, alphaSourceBlendMode alphaDestBlendMode
BlendOp colorOp
BlendOp colorOp, alphaOp
AlphaToMask On | Off
````
设置 Alpha 混合、Alpha 操作和 alpha-to-coverage 模式。请参阅有关[混合](SL-Blend.html)的文档以了解更多详细信息。


###ColorMask
````
ColorMask RGB | A | 0 | R、G、B、A 的任意组合
````
设置颜色通道写入遮罩。写入 _ColorMask 0_ 可关闭对所有颜色通道的渲染。默认模式是
写入所有通道 (RGBA)，但是对于某些特殊效果，您可能希望不修改某些通道，或完全禁用颜色
写入。

使用多渲染目标 (MRT) 渲染时，可通过在末尾添加索引（0 到 7）来为每个渲染目标设置不同的颜色遮罩。例如，`ColorMask RGB 3` 将使渲染目标 #3 仅写入到 RGB 通道。

### 旧版固定函数着色器命令

一些命令用于编写旧版“固定函数样式”着色器。这是视为已弃用的功能，因为编写[表面着色器](SL-SurfaceShaders.html)或[着色器程序](SL-ShaderPrograms.html)
可带来更大的灵活性。但是，对于非常简单的着色器，以固定函数样式编写着色器有时会更容易，因此这里提供了命令。请注意，如果不使用固定函数着色器，则会忽略以下所有命令。

#### 固定函数 Lighting 和 Material
````
Lighting On | Off
Material { Material Block }
SeparateSpecular On | Off
Color Color-value
ColorMaterial AmbientAndDiffuse | Emission
````
所有这些均控制固定函数每顶点光照：它们将其开启，设置材质颜色，开启镜面高光，提供默认颜色（如果顶点光照关闭），并控制网格顶点颜色如何影响光照。请参阅有关[材质](SL-Material.html)的文档以了解更多详细信息。


#### 固定函数 Fog
````
Fog { Fog Block }
````
设置固定函数 Fog 的参数。请参阅有关[雾化](SL-Fog.html)的文档以了解更多详细信息。


#### 固定函数 AlphaTest
````
AlphaTest (Less | Greater | LEqual | GEqual | Equal | NotEqual | Always) CutoffValue
````
开启固定函数 Alpha 测试。请参阅有关 [Alpha 测试](SL-AlphaTest.html)的文档以了解更多详细信息。


#### 固定函数纹理组合器

设置渲染状态后，可使用 [SetTexture](SL-SetTexture.html) 命令指定多个纹理及其组合模式：

````
SetTexture textureProperty { combine options }
````

## 详细信息

着色器通过几种方式与 Unity 的渲染管线进行交互；例如，通道可以使用 [Tags](SL-PassTags.html) 命令来表明它只应该用于[延迟着色](RenderTech-DeferredShading.html)。某些通道也可以在同一个游戏对象上执行多次；例如，在[前向渲染](RenderTech-ForwardRendering.html)中，根据影响游戏对象的
光源数量，将多次执行“ForwardAdd”通道类型。请参阅有关[渲染管线](SL-RenderPipeline.html)的文档以了解更多详细信息。


## 另请参阅

有几个特殊的通道可用于重复使用通用功能或实现各种高端效果：

* [UsePass](SL-UsePass.html) 包含来自另一个着色器的指定通道。
* [GrabPass](SL-GrabPass.html) 将屏幕内容抓取到纹理中，以便在之后的通道中使用。


