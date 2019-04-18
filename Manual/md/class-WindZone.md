# 树 - 风区


__风区__使创建的树挥动树枝和树叶，仿佛被风吹动一样，因此能为树添加真实感。


![左侧为球形风区，右侧为定向风区。](../uploads/Main/InspectorWindZones.png)

## 属性


|**_属性：_** |**_功能：_** |
|:---|:---|
|__Mode__ ||
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Spherical__ |风区仅在半径内有效果，并且从中心向边缘衰减。|
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;__Directional__ |风区在一个方向上影响整个场景。|
|__Radius__ |球形风区 (Spherical Wind Zone) 的半径（仅在模式设置为 Spherical 时才有效）。|
|__Main__ |主风力。产生轻微变化的风压。|
|__Turbulence__ |强风气流。产生快速变化的风压。|
|__Pulse Magnitude__ |定义风随时间变化的程度。|
|__Pulse Frequency__ |定义风的变化频率。|

## 详细信息

__风区__仅由树创建器用于对树叶和树枝添加动画。这有助于让场景看起来更自然，并允许游戏中的作用力（例如爆炸）看起来像在与树木相互作用。
有关树的工作原理的更多信息，请访问[树类页面](class-Tree.html)。

##在 Unity 中使用风区

在 Unity 中使用__风区__十分简单。
首先，要新建__风区__，只需单击 __Game Object &gt; 3D Object &gt; Wind Zone__。

将风区（取决于类型）放置在使用[树创建器](class-Tree.html)创建的树附近，并观察风区与树的相互作用！

**注意：**如果风区为球形风区，放置风区时应让被风吹的树位于球体半径内。如果风区为定向风区，则风区在场景中的放置位置并不重要。

##提示



* 要产生轻微变化的普通风，请执行以下操作：
    * 创建定向风区。
    * 根据风应有的强弱程度，将 Wind Main 设置为 1.0 或更小值。
    * 将 Turbulence 设置为 0.1。
    * 将 Pulse Magnitude 设置为 1.0 或更大值。
    * 将 Pulse Frequency 设置为 0.25。


* 要创建直升机飞过的效果，请执行以下操作：
    * 创建球形风区。
    * 将 Radius 设置为适合直升机大小的数值
    * 将 Wind Main 设置为 3.0
    * 将 Turbulence 设置为 5.0
    * 将 Pulse Magnitude 设置为 0.1
    * 将 Pulse Frequency 设置为 1.0
    * 将风区附加到表示直升机的游戏对象。


* 要创造爆炸效果，请执行以下操作：
    * 与直升机一样操作，但应快速淡化 Wind Main 和 Turbulence，使效果逐渐消失。

---

* <span class="page-edit">2017-09-19  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 Unity 4.6 中更改了 GameObject 菜单</span>
