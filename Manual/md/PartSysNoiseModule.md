# Noise 模块

使用此模块可为粒子移动添加湍流。

![](../uploads/Main/PartSysNoiseModule.png) 

## 属性

| 属性| 功能 |
|:---|:---| 
| __Separate Axes__| 在每个轴上独立控制强度和重新映射。 |
| __Strength__| 通过一条曲线定义噪声在粒子的生命周期内对粒子的影响有多强。值越高，粒子移动越快和越远。 |
| __Frequency__| 低值会产生柔和、平滑的噪声，而高值会产生快速变化的噪声。此属性可控制粒子改变行进方向的频率以及方向变化的突然程度。 |
| __Scroll Speed__| 随着时间的推移而移动噪声场可产生更不可预测和不稳定的粒子移动。 |
| __Damping__| 启用此属性后，强度与频率成正比。将这些值绑在一起意味着可在保持相同行为但具有不同大小的同时缩放噪声场。 |
| __Octaves__| 指定组合多少层重叠噪声来产生最终噪声值。使用更多层可提供更丰富、更有趣的噪声，但会显著增加性能成本。 |
| __Octave Multiplier__| 对于每个附加的噪声层，按此比例降低强度。 |
| __Octave Scale__| 对于每个附加的噪声层，按此乘数调整频率。 |
| __Quality__| 较低的质量设置可显著降低性能成本，但也会影响噪声的有趣程度。请使用能为您提供所需行为的最低质量以获得最佳性能。 |
| __Remap__| 将最终噪声值重新映射到不同的范围。 |
| __Remap Curve__| 描述最终噪声值如何变换的曲线。例如，可使用此选项来创建从高点开始并以零结束的曲线，从而选择噪声场的较低范围并忽略较高范围。 |
| __Position Amount__| 用于控制噪声对粒子位置影响程度的乘数。 |
| __Rotation Amount__| 用于控制噪声对粒子旋转（以度/秒为单位）影响程度的乘数。 |
| __Size Amount__| 用于控制噪声对粒子大小影响程度的乘数。 |

## 详细信息

为粒子添加噪声是创建有趣方案和效果的简单有效方法。例如，想象一下火焰中的余烬是如何移动的，或者烟雾在移动时是如何旋转的。强烈的高频噪声可用于模拟火焰余烬，而柔和的低频噪声更适合模拟烟雾效果。

为了最大程度控制噪声，可启用 Separate Axes 选项。此选项允许您在每个轴上独立控制强度和重新映射。

使用的噪声算法基于一种称为“卷曲噪声”(Curl Noise) 的技术，而该技术在内部使用多个柏林噪声 (Perlin Noise) 样本来创建最终噪声场。

质量设置控制着生成的独特噪声样本数量。使用 Medium 和 Low 设置时，使用的柏林噪声样本较少，这些样本将在多个轴上重用，但会组合在一起以尽可能进行重用并隐藏这样的重用。这意味着当使用较低质量的设置时，噪声可能看起来不那么动态和多样化。但是，使用较低质量的设置时，可以获得显著的性能优势。

----
*  <span class="page-edit">2017-09-05  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>

*  <span class="page-history">在 Unity [2017.1](https://docs.unity3d.com/2017.1/Documentation/Manual/30_search.html?q=newin20171) 中添加了 Position Amount、Rotation Amount 和 Size Amount <span class="search-words">NewIn20171</span></span>
*  <span class="page-history">在 Unity [2017.2](https://docs.unity3d.com/2017.2/Documentation/Manual/30_search.html?q=newin20172) 中添加了 Strength、Frequency、噪声算法和质量设置 <span class="search-words">NewIn20172</span></span>

