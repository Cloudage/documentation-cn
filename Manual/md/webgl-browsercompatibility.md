#WebGL 浏览器兼容性

Unity WebGL 在某种程度上支持所有主流桌面浏览器。但是，不同浏览器的支持程度和预期性能会有所不同。请参阅下表，了解 Unity WebGL 内容相关的浏览器功能以及支持这些功能的浏览器。

请注意，移动设备目前不支持 Unity WebGL 内容。Unity WebGL 内容可能仍然有效，特别是在高端设备上，但许多当前设备的性能不够强大，也没有足够的内存来充分支持该内容。因此，Unity WebGL 尝试在移动端浏览器上加载内容时会显示警告消息（如果需要，可禁用警告）。

请注意，此兼容性表对于所述的具体浏览器版本有效。支持能力在将来的版本中应该会更进一步，但在以前的版本中可能不稳定。

|**桌面浏览器兼容性表** |||||||
|:---|:---|:---|:---|:---|:---|:---|
||**Mozilla Firefox 52**|**Google Chrome 57**|**Apple Safari 11**|**MS Edge 16**||
|**WebGL 支持**|**是** <br/> GPU 黑名单适用。特定的旧显卡可能不支持 WebGL。有关详细信息，请参阅 [Mozilla wiki 页面：阻止列表/被阻止的图形驱动程序 (Blocklisting/Blocked Graphics Drivers)](https://wiki.mozilla.org/Blocklisting/Blocked_Graphics_Drivers) 和 [Khronos wiki 页面：黑名单和白名单 (Blacklists and Whitelists)](https://www.khronos.org/webgl/wiki/BlacklistsAndWhitelists)。|**是** <br/> GPU 黑名单适用。特定的旧显卡可能不支持 WebGL。有关详细信息，请参阅 [Mozilla wiki 页面：阻止列表/被阻止的图形驱动程序 (Blocklisting/Blocked Graphics Drivers)](https://wiki.mozilla.org/Blocklisting/Blocked_Graphics_Drivers) 和 [Khronos wiki 页面：黑名单和白名单 (Blacklists and Whitelists)](https://www.khronos.org/webgl/wiki/BlacklistsAndWhitelists)。|**是** <br/> Safari 8 和更高版本|**是**|
|**Web 音频** <br/>（请参阅 [Web 音频](webgl-audio.html)）<br/>必须具备 Web 音频 API 才能在 Unity WebGL 内容中播放声音。|**是**|**是**|**是**|**是**|
|**全屏支持** <br/>（请参阅[全屏支持](webgl-cursorfullscreen.html)）|**是**|**是**|**是**<br/>Safari 10.1 或更高版本|**是**|
|**光标锁定支持** <br/>（请参阅[光标锁定支持](webgl-cursorfullscreen.html)）|**是**|**是**|**是**|**是** <br/> Edge 13 和更高版本。|
|**游戏手柄支持** <br/>（请参阅[游戏手柄支持](webgl-input.html)）|**是**|**是**|**是**|**是**|
|**IndexedDB** <br/>对于数据缓存功能、[PlayerPrefs](../ScriptReference/PlayerPrefs.html) 类和 [WWW.LoadFromCacheOrDownload](../ScriptReference/WWW.LoadFromCacheOrDownload.html) 使用的本地存储而言是必需的。|**是** <br/> Firefox 42 及更早版本对于在 iFrame 中运行的内容不支持 IndexedDB。Firefox 43 和更高版本修复了此问题。|**是**|**是** <br/> Safari 对于在 iFrame 中运行的内容不支持 IndexedDB。|**是**|
|**WebSockets** <br/>对于[网络](UNet.html)而言是必需的。 |**是**|**是**|**是**|**是**|
|**WebRTC** <br/>对于 [WebCamTexture](../ScriptReference/WebCamTexture.html) 类而言是必需的。|**是**|**是**|**否**|**是**|
|**WebGL 2.0** <br/>（请参阅 [WebGL 2.0](webgl-graphics.html)）|**是** <br/> Firefox 51 和更高版本|**是** <br/> Chrome 56 和更高版本|**否**|**否**|
|**asm.js AOT 编译** <br/>asm.js 是浏览器可专门优化的一个 JavaScript 子集。实现 asm.js 支持的浏览器也许能够更快地运行 Unity WebGL 内容，因为 Unity 使用 asm.js。|**是**|**否**|**否**|**是**|
|**WebAssembly** <br/>WebAssembly 或 wasm 是一种适合编译到 Web 的可移植、占用空间小且加载速度快的新格式。|**是** <br/> Firefox 52 和更高版本。|**是** <br/> Chrome 57 和更高版本。|**是**<br/>Safari 11 或更高版本|**是**<br/>Edge 16 或更高版本|
|**Large-Allocation Http 标头** <br/>帮助浏览器确保有足够的内存可用于加载内容（请参阅 [Large-Allocation Http 标头](webgl-memory.html)）|**是** <br/> Firefox 53 和更高版本。|**否**|**否**|**否**|
|**Brotli 压缩** <br/>减少构建大小（请参阅 [Brotli 压缩](webgl-deploying.html)）|**是**|**是**|**否**|**是**|
##注意

* Chrome 可能需要大量内存来解析生成的 JavaScript 代码，这可能会导致在 32 位浏览器上加载内容时出现内存不足错误或崩溃。有关内存使用情况的更多信息，请参阅[内存注意事项](webgl-memory.html)。

<br/><br/> 

-----

*  <span class="page-edit">2017-05-15  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span>

*  <span class="page-history">在用户手册 5.6 中的此页面上首次添加了关于 Brotli 压缩的文档</span>
