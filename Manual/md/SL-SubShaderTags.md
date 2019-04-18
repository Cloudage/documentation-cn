#ShaderLab：子着色器标签 (SubShader Tags)

子着色器使用标签来告知它们期望何时以何种方式被渲染到渲染引擎。

## 语法

````
Tags { "TagName1" = "Value1" "TagName2" = "Value2" }
````

指定 **TagName1** 具有 **Value1**，**TagName2** 具有 **Value2**。可根据需要添加任意数量的标签。


## 详细信息


从根本上说，标签就是键/值对。在 [SubShader](SL-SubShader.html) 中，标签用于确定子着色器的渲染顺序和其他参数。请注意，以下由 Unity 识别的标签**必须**位于 SubShader 部分中，不能在 Pass 中！

除了 Unity 可以识别的内置标签外，您还可以使用自己的标签并使用 [Material.GetTag](../ScriptReference/Material.GetTag.html) 函数来查询这些标签。


### 渲染顺序 - Queue 标签

您可以使用 _Queue_ 标签来确定对象的绘制顺序。着色器决定其对象属于哪个渲染队列，这样任何透明着色器都可以确保它们在所有不透明对象之后绘制，依此类推。

有四个预定义的渲染队列，但预定义的渲染队列之间可以有更多的队列。预定义队列包括：

* `Background` - 此渲染队列在任何其他渲染队列之前渲染。通常会对需要处于背景中的对象使用此渲染队列。
* `Geometry`_（默认值）_- 此队列用于大部分对象。不透明几何体使用此队列。
* `AlphaTest` - 进行 Alpha 测试的几何体将使用此队列。这是不同于 `Geometry` 队列的单独队列，因为在绘制完所有实体对象之后再渲染经过 Alpha 测试的对象会更有效。
* `Transparent` - 此渲染队列在 _Geometry_ 和 `AlphaTest` 之后渲染，按照从后到前的顺序。任何经过 Alpha 混合者（即不写入深度缓冲区的着色器）都应该放在这里（玻璃、粒子效果）。
* `Overlay` - 此渲染队列旨在获得覆盖效果。最后渲染的任何内容都应该放在此处（例如，镜头光晕）。



````
Shader "Transparent Queue Example"
{
     SubShader
     {
        Tags { "Queue" = "Transparent" }
        Pass
        {
            // 着色器主体的剩余部分...
        }
    }
}
````
_说明如何在透明队列中渲染对象的示例_

对于特殊用例，可以使用中间队列。在内部，每个队列由整数索引表示；`Background` 是 1000，`Geometry` 是 2000，`AlphaTest` 是 2450，`Transparent` 是 3000，`Overlay` 是 4000。如果着色器使用如下所示的队列：

````
Tags { "Queue" = "Geometry+1" }
````

这将使对象在所有不透明对象之后渲染，但在透明对象之前渲染，因为渲染队列索引值将是 2001（几何体索引值加一）。如果您希望始终在其他对象集之间绘制某些对象，那么这非常有用。例如，在大多数情况下，透明的水应该在不透明对象之后绘制，但又要在透明对象之前绘制。

索引值高达 2500 的队列（“Geometry+500”）视为“不透明”，并且会优化对象的绘制顺序以获得最佳性能。
较高索引的渲染队列被认为是“透明对象”并按距离将对象进行排序，从距离最远的对象开始渲染，最后渲染距离最近的对象。天空盒在所有不透明对象和所有透明对象之间绘制。



###RenderType 标签

`RenderType` 标签将着色器分为几个预定义的组，例如，不透明的着色器，或经过 Alpha 测试的着色器等等。[着色器替换](SL-ShaderReplacement.html)将使用该标签，在某些情况下还用于产生[摄像机的深度纹理](SL-CameraDepthTexture.html)。


###DisableBatching 标签

使用[绘制调用批处理](DrawCallBatching.html)时，一些着色器（主要是进行对象空间顶点变形的着色器）不起作用，这是因为批处理会将所有几何体转换为世界空间，所以“对象空间”丢失。

可使用 `DisableBatching` 标签来指示这一情况。有三个可能的值：“True”（始终对此着色器禁用批处理）、“False”（不禁用批处理；这是默认值）和“LODFading”（当 LOD 淡化处于激活状态时禁用批处理；主要用于树）。


###ForceNoShadowCasting 标签

如果提供了 `ForceNoShadowCasting` 标签并且值为“True”，则使用该子着色器渲染的对象绝不会投射阴影。对透明对象使用着色器替换并且不希望从其他子着色器继承阴影通道时，这非常有用。

###IgnoreProjector 标签

如果提供了 `IgnoreProjector` 标签并且值为“True”，则使用此着色器的对象不会受到[投影器](class-Projector.html)的影响。这对半透明对象非常有用，因为投影器无法影响它们。

###CanUseSpriteAtlas 标签

如果着色器用于精灵，请将 `CanUseSpriteAtlas` 标签设置为“False”，这样在精灵打包到图集内时，该标签将不起作用，（请参阅[精灵打包器 (Sprite Packer)](SpritePacker.html)）。


###PreviewType 标签

`PreviewType` 指示材质检视面板预览应如何显示材质。默认情况下，材质显示为球体，但也可以将 PreviewType 设置为“Plane”（将显示为 2D）或“Skybox”（将显示为天空盒）。


##另请参阅

也可以为通道提供标签，请参阅[通道标签](SL-PassTags.html)。
