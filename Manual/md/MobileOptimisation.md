优化
=============


就像在 PC 上一样，iOS 和 Android 等移动平台拥有各种性能水平的设备。可轻松找到比其他手机功能强大 10 倍的手机。
非常简单的扩展方式：

1.确保在基线配置上运行正常
1.在性能更高的配置上使用更多视觉效果：
    * 分辨率
    * 后期处理
    * MSAA
    * 各向异性
    * 着色器
    * Fx/粒子密度，开/关

关注 GPU
-------------


图形性能受填充率、像素和几何复杂度（顶点数）限制。如果能找到一种方法来剔除更多渲染器，所有这三种因素都可以减少。这种情况下，遮挡剔除可能会很有作用，因为 Unity 会自动剔除视锥体外的对象。

在移动平台，本质上将受填充率限制（填充率 = 屏幕像素 * 着色器复杂度 * 过度绘制），而过于复杂的着色器是导致问题的最常见原因。因此，请使用 Unity 附带的移动端着色器，或设计您自己的着色器，但尽可能简化它们。如果可能，通过将代码移动到顶点着色器来简化像素着色器。

如果降低质量设置 (Quality Settings) 中的纹理质量 (Texture Quality) 能提高游戏运行速度，表明可能受到内存带宽的限制。因此，应压缩纹理、使用 Mipmap、减小纹理大小等。

LOD（细节级别）：将对象简化或在对象移动到更远位置时完全剔除它们。

###良好实践

移动端 GPU 在产生的热量、使用的功率以及可能的大小或噪声等方面都有很大的约束。因此，与桌面端部件相比，移动端 GPU 具有低得多的带宽、更低 ALU 性能和更弱的纹理功能。GPU 的架构也经过调整，旨在尽可能减少带宽和功率。

Unity 针对 OpenGL ES 2.0 进行了优化，采用 GLSL ES（类似于 HLSL）着色语言。内置着色器通常用 HLSL（也称为 Cg）编写而成。对于移动平台，这些着色器将交叉编译为 GLSL ES。如果需要，也可直接编写 GLSL，但这样做会局限于 OpenGL 类平台（例如，移动端 + Mac），因为目前没有 GLSL &gt; HLSL 转换工具。在 HLSL 中使用 float/half/fixed 类型时，它们最终会在 GLSL ES 中生成 highp/mediump/lowp 精度限定符。

以下是确保良好实践的检查清单：

1.保持材质数量尽可能少。这样使 Unity 更容易进行批量处理。
1.使用纹理图集（包含子图像集合的大图像）代替多个单独的纹理。这些纹理图集的加载速度更快，状态切换更少，并支持批处理。
1.如果使用纹理图集和共享材质，请用 _Renderer.sharedMaterial_ 而不是 _Renderer.material_。
1.前向渲染的像素光源的成本很高。
    * 尽可能使用光照贴图而不是实时光源。
    * 调整质量设置中的像素光源数。本质上只有方向光应该是每个像素光源，其他所有光源都应该是每顶点光源。当然这取决于游戏。
1.尝试质量设置中的光源渲染模式，以获取正确的优先级。
1.除非确实有必要，否则避免使用镂空（Alpha 测试）着色器。
1.保持透明（Alpha 混合）屏幕覆盖率最小化。
1.尽量避免多个光源为给定对象提供光照的情况。
1.尝试减少着色器通道（阴影、像素光源、反射）的总数。
1.渲染顺序至关重要。一般情况为：
    * 完全不透明对象（大致为从前到后）。
    * Alpha 测试对象（大致为从前到后）。
    * 天空盒。
    * Alpha 混合对象（必要时从后到前）。
1.后期处理在移动端的成本很高，请小心使用。
1.粒子：减少过度绘制，使用尽可能最简单的着色器。
1.使每帧都修改的网格的缓冲区加倍：


````
void Update (){
  // 在网格之间翻转
  bufferMesh = on ? meshA : meshB;
  on = !on;
  bufferMesh.vertices = vertices; // 修改网格
  meshFilter.sharedMesh = bufferMesh;
}
````

###着色器优化

检查是否收到填充率限制很容易：如果降低显示分辨率，游戏运行速度是否会加快？如果是，表明受限于填充率。

尝试通过以下方法降低着色器复杂性：

* 避免使用 Alpha 测试着色器；改用 Alpha 混合版本。
* 使用简单、优化的着色器代码（例如 Unity 附带的“Mobile”着色器）。
* 避免在着色器代码中使用成本太高的数学函数（pow、exp、log、cos、sin、tan 等）。请考虑改用预先计算的查找纹理。
* 选择尽可能低的数字精度格式（float、half、fixedin Cg）以获得最佳性能。

关注 CPU
-------------


通常情况下，游戏在像素处理方面受限于 GPU。因此，这些游戏最终还有未使用的 CPU 处理能力，尤其是在多核移动端 CPU 上。因此，通常情况下将一些工作从 GPU 转移到 CPU 是明智的做法（由 Unity 执行所有这些转移操作），如以下工作：网格蒙皮、小对象的批处理、粒子几何体更新。

这些方法应谨慎使用，而不是盲目进行。如果未受到绘制调用的约束，那么批处理实际上对性能更不利，因为这种处理方式会降低剔除效率并使更多对象受到光源的影响！

###良好实践


* FindObjectsOfType（以及一般的 Unity getter 属性）非常慢，因此请合理使用它们。
* 在非移动对象上设置 Static 属性以便允许内部优化，如静态批处理。
* 将大量 CPU 周期用于进行遮挡剔除和更好的排序（以便利用 Early Z-cull）。

###物理组件

物理组件可能很消耗 CPU。可通过 Editor 性能分析器对其进行分析。如果物理组件在 CPU 上占用太多时间：

* 尽可能将 _Time.fixedDeltaTime_（在 Project settings &gt; Time 中）调整到允许的最大值。如果游戏的移动速度很慢，所需的固定更新可能少于具有快动作的游戏。快节奏的游戏需要更频繁的计算，因此 _fixedDeltaTime_ 将需要更低的值，否则碰撞可能失败。
* Physics.solverIterationCount (Physics Manager)。
* 使用尽可能少的布料 (Cloth) 对象。
* 仅在必要时才使用刚体。
* 优先使用原始碰撞体而不是网格碰撞体。
* 绝不要移动静态碰撞体（即没有刚体的碰撞体），因为这会导致很大的性能影响。这种碰撞体在性能分析器中显示为“Static Collider.Move”，但实际处理在 _Physics.Simulate_ 中进行。如有必要，添加刚体，并将 _isKinematic_ 设置为 true。
* 在 Windows 上，如果需要，可使用 NVidia 的 AgPerfMon 性能分析工具集来获取更多详细信息。

##Android
###GPU

以下是流行的移动平台架构。移动平台的硬件供应商不同于 PC/游戏主机领域，GPU 架构也非常不同于“常规”GPU。

* ImgTec PowerVR SGX - 基于区块、延迟：以小区块（16x16 大小）渲染所有内容，仅对可见像素着色
* NVIDIA Tegra - 经典：渲染所有内容
* Qualcomm Adreno - 区块：以区块渲染所有内容，采用大区块（256k 大小）设计。Adreno 3xx 可切换到传统模式。
* ARM Mali - 区块：以区块渲染所有内容，采用小区块（16x16 大小）设计。

花一些时间研究不同的渲染方法，并相应地设计游戏。特别要注意排序。在开发周期的早期定义支持的最低端设备。在设计游戏时使用性能分析器在这些设备上进行测试。

使用特定于平台的纹理压缩格式。

###阅读更多信息

* PowerVR SGX 架构指南：[http://imgtec.com/powervr/insider/powervr-sdk-docs.asp](http://imgtec.com/powervr/insider/powervr-sdk-docs.asp)
* Tegra GLES2 功能指南：[http://developer.download.nvidia.com/assets/mobile/files/tegra_gles2_development.pdf](http://developer.download.nvidia.com/assets/mobile/files/tegra_gles2_development.pdf)
* Qualcomm Adreno GLES 性能指南：[http://developer.qualcomm.com/file/607/adreno200performanceoptimizationopenglestipsandtricksmarch10.pdf](http://developer.qualcomm.com/file/607/adreno200performanceoptimizationopenglestipsandtricksmarch10.pdf)
* Engel、Rible：[http://altdevblogaday.com/2011/08/04/programming-the-xperia-play-gpu-by-wolfgang-engel-and-maurice-ribble/](http://altdevblogaday.com/2011/08/04/programming-the-xperia-play-gpu-by-wolfgang-engel-and-maurice-ribble/)
* [ARM Mali GPU 优化指南](https://developer.arm.com/products/software-development-tools/graphics-development-tools/mali-graphics-debugger/docs/100140/0303)

###屏幕分辨率

###Android 版本

##iOS

###GPU

只需要关注 PowerVR 架构（基于区块、延迟）。

* ImgTec PowerVR SGX。基于区块、延迟：以区块渲染所有内容，仅对可见像素着色。

这意味着：

* Mipmap 不是那么有必要。
* 抗锯齿和各向异性的成本足够低，某些情况下在 iPad 3 上不需要。

缺点：

* 如果每帧的顶点数据（顶点数 * 顶点着色器之后所需的存储空间）超出驱动程序分配的内部缓冲区，则必须“拆分”场景，而这会降低性能。在此之后，驱动程序可能会分配更大的缓冲区，或者您可能需要减少顶点数。在 iPad2 (iOS 4.3) 上，当大约有 10 万个顶点且具有非常复杂的着色器时，此情况将变得明显。
* TBDR 需要为区块部分和延迟部分分配更多的晶体管，从而在概念上留给“原始性能”的晶体管将减少。为 TBDR 上的绘制调用获取 GPU 时序非常困难（几乎不可能），导致分析变得困难。

###阅读更多信息

* PowerVR SGX 架构指南：[http://imgtec.com/powervr/insider/powervr-sdk-docs.asp](http://imgtec.com/powervr/insider/powervr-sdk-docs.asp)

###屏幕分辨率

###iOS 版本

动态对象
---------------


###资源包


* 资源包在一定限制程度上缓存于设备上
* 使用 Editor API 进行创建
* 使用 WWW API 进行加载：WWW.LoadFromCacheOrDownload，或作为资源加载：AssetBundle.CreateFromMemory 或 AssetBundle.CreateFromFile
* 使用 AssetBundle.Unload 进行卸载。可选择卸载资源包但保留已加载的资源。即使已加载的资源在场景中被引用，也可以终止所有这些资源
* Resources.UnloadUnusedAssets 卸载场景中不再引用的所有资源。所以，切记终止对不再需要的资源的引用。系统绝不会对公共变量和静态变量进行垃圾回收。
* Resources.UnloadAsset 从内存中卸载特定资源。如果需要，可从磁盘重新加载该资源。


####在 iOS 上对 AssetBundle 的同时下载数量是否有任何限制？（例如，是否可同时（或每一帧）安全下载超过 10 个 AssetBundle？)


下载是通过操作系统提供的异步 API 实现的，因此由操作系统决定需要为下载创建多少个线程。启动多个并发下载时，应注意可支持的总设备带宽和可用内存量。每个并发下载都会分配自己的临时缓冲区，因此应注意不要耗尽内存。

###资源



* 资源需要经过 Unity 识别后才能置于构建中。
* 将 .bytes 文件扩展名添加到您希望 Unity 识别为二进制数据的任何原始字节。
* 将 .txt 文件扩展名添加到您希望 Unity 识别为文本资源的任何文本文件
* 资源在构建时转换为平台格式。
* Resources.Load()

问题检查清单
----------------------




* 未经正确压缩的纹理
* 不同情况有不同解决方案，但一定要压缩纹理，除非您确定不应该压缩。
* ETC/RGBA16 - Android 的默认设置，但可根据 GPU 供应商做调整。最好的方法是尽可能使用 ETC。Alpha 纹理可使用两个 ETC 文件，并将一个通道用于 Alpha
* PVRTC - iOS 的默认设置，适用于大多数情况
* 启用了 Get/Set 像素的纹理 - 占用空间加倍，除非需要 Get/Set，否则应取消选中
* 在运行时从 JPEG/PNG 加载的纹理将是未压缩的
* 大型 mp3 文件在加载时标记为解压缩
* 附加场景加载
* 未使用的资源保留在内存中未清除。
* 如果发生随机崩溃，请尝试使用开发工具包或搭载 2 GB 内存的设备（如 Ipad 3）。


有时控制台中不显示任何信息，只是发生随机崩溃

* 快速脚本调用和剥离可能会导致 iOS 上的随机崩溃。尝试不使用这些功能。
