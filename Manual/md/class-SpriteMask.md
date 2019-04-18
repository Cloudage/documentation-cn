# 精灵遮罩

精灵遮罩用于隐藏或显示精灵或精灵组的各个部分。精灵遮罩仅影响使用精灵渲染器 (Sprite Renderer) 组件的对象。

### 创建精灵遮罩

要创建精灵遮罩，请从主菜单选择：GameObject > 2D Object > Sprite Mask。

![从菜单创建精灵遮罩](../uploads/Main/SpriteMask1.png)


![已在场景中创建新的精灵遮罩游戏对象](../uploads/Main/SpriteMask2.png)


### 属性

![](../uploads/Main/SpriteMask3.png) 

| 属性| 功能 |
|:---|:---| 
| Sprite| 要用作遮罩的精灵。|
| Alpha Cutoff| 如果 Alpha 包含透明区域和不透明区域之间的混合，则可以手动确定所显示区域的分界点。通过调整 Alpha Cutoff 滑动条即可更改此分界点。|
| __Range Start__ | Range Start 是遮罩开始遮蔽时所在的排序图层 (Sorting Layer)。|
| Sorting Layer| 遮罩的排序图层。|
| Order in Layer| 排序图层中的顺序。|
| __Range End__ |  |
| Mask All| 默认情况下，此遮罩将影响其后（更低排序顺序）的所有排序图层。|
| Custom| Range End 可以设置为自定义 Sorting Layer 和 Order in Layer。|



### 使用精灵遮罩

![需要将用作遮罩的精灵分配给精灵遮罩 (Sprite Mask) 组件](../uploads/Main/SpriteMask4.png)

精灵遮罩游戏对象本身将在场景中不可见，只有与精灵产生的交互才可见。要查看场景中的精灵遮罩，请选择 Scene 菜单中的 Sprite Mask 选项。

![Scene 视图以及场景中已开启的 Sprite Mask 视图](../uploads/Main/SpriteMask5.jpg)

精灵遮罩始终有效。受精灵遮罩影响的精灵需要在 Sprite Renderer 中设置 Mask Interaction。

![角色精灵的 Mask Interaction 设置为 Visible Under Mask，因此只显示处于遮罩区域中的精灵部分](../uploads/Main/SpriteMask6.jpg)


默认情况下，精灵遮罩会影响场景中将 Mask Interaction 设置为 Visible 或 Not Visible Under Mask 的所有精灵。我们通常希望遮罩仅影响特定精灵或一组精灵。

![角色精灵正在与两张卡上的遮罩交互](../uploads/Main/SpriteMask7.jpg)

确保遮罩与特定精灵交互的一种方法是使用排序组 (Sorting Group) 组件。

![添加到父游戏对象的排序组 (Sorting Group) 组件确保遮罩仅影响该排序组的子项](../uploads/Main/SpriteMask8.jpg)

控制遮罩效果的另一种方法是使用精灵遮罩的自定义范围 (Custom Range) 设置。

![具有自定义范围 (Custom Range) 设置的精灵遮罩确保遮罩仅会影响指定的 Sorting Layer 或 Order in Layer 范围内的精灵](../uploads/Main/SpriteMask9.jpg)



Range Start 和 Range End 可以基于精灵的 Sorting Layer 或 Order in Layer 来选择性遮蔽精灵。

![](../uploads/Main/SpriteMask10.jpg) 

<br/><br/> 

----

* <span class="page-edit">2017-05-26 Page published with no [editorial review](DocumentationEditorialReview.html)
</span>
 
* <span class="page-history">Unity [2017.1](../Manual/30_search.html?q=newin20171) 中的新功能 <span class="search-words">NewIn20171</span></span>

