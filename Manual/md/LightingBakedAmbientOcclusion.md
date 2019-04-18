# 烘焙环境光遮挡

__环境光遮挡__ (Ambient occlusion, AO) 可估算环境光照（不是来自特定方向的光照）照射到表面上某一点的光照的情况。它使彼此接近的折痕、孔和表面变暗。这些区域将遮挡（阻挡）环境光，因此它们看起来较暗。

仅使用预计算实时 GI 时（请参阅有关[使用预计算光照](UsingPrecomputedLighting.html)的文档），间接光照的部分不会捕获精细细节或动态对象。我们建议使用实时环境光遮挡后期效果，它具有更多细节，可以生成更高质量的光照。

要查看和启用烘焙 AO，请打开 __Lighting__ 窗口（菜单：__Window__ > __Lighting__），然后找到 __Baked GI__ 部分。勾选 __Baked GI__ 复选框（如果未选中），然后勾选 __Ambient Occlusion__ 复选框以启用烘焙 AO。

要查看和启用烘焙 AO，请打开 __Lighting__ 窗口（菜单：__Window__ > __Lighting__），然后导航到 __Mixed Lighting__ 部分。勾选 __Baked Global Illumination__ 复选框（如果未选中），然后在 __Lightmapping Settings__ 中勾选 __Ambient Occlusion__ 复选框以启用烘焙 AO。


![](../uploads/Main/BakedAO56.png) 


| **属性** | **功能** |
|:---|:---|
| __Max Distance__ | 使用此属性可设置光线在终止前被跟踪的距离值。 |
| __Indirect__ | 使用此属性可控制 AO 对间接光的影响程度。滑动条值设置得越高，折痕、孔和近表面被间接光照射时的外观就越暗。<br/><br/>更加逼真的做法是仅将 AO 应用于间接光照。 |
| __Direct__ | 使用此属性可控制 AO 对直射光的影响程度。滑动条值设置得越高，场景的折痕、孔和近表面被直射光照射时的外观就越暗。<br/><br/>默认情况下，AO 不会影响直接光照。使用此滑动条即可启用这一属性。该特效不真实，但可用于艺术效果。|


有关实时环境光遮挡的最新实现方式，请参阅有关[环境光遮挡](PostProcessing-AmbientOcclusion.html)后期处理效果的文档。

要更详细了解 AO，请参阅 [Wikipedia：环境光遮挡 (Ambient Occlusion)](http://en.wikipedia.org/wiki/Ambient_occlusion)。
