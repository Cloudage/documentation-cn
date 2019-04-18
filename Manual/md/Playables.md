#Playables API

Playables API 提供一种通过组织和评估树状结构（称为 PlayableGraph）中的数据源来创建工具、效果或其他游戏机制的方法。PlayableGraph 允许您混合、融合和修改多个数据源，并通过单个输出播放它们。
 
Playables API 支持动画、音频和脚本。Playables API 还提供通过脚本与[动画系统](AnimationSection.html)和音频系统进行交互的能力。
 
尽管 Playables API 目前仅限于动画、音频和脚本，但它是一种通用 API，最终可供视频和其他系统使用。

##可播放项 (Playable) 与动画组件

动画系统已有一个图形编辑工具，这是一个仅限于播放动画的状态机系统。Playables API 设计得更灵活并支持其他系统。Playables API 还可创建状态机无法实现的图形。这些图形表示一个数据流，指示每个节点生成和使用的内容。此外，单个图形不限于单个系统。单个图形可能包含动画、音频和脚本的节点。

##Playables API 的优点

* Playables API 支持动态动画混合。这意味着场景中的对象可以提供自己的动画。例如，武器、宝箱和陷阱的动画可以动态添加到 PlayableGraph 并使用一段时间。

* Playables API 可让您轻松播放单个动画，而不会产生创建和管理 AnimatorController 资源所涉及的开销。

* Playables API 允许用户动态创建混合图并直接逐帧控制混合权重。

* 可在运行时创建 PlayableGraph，根据条件按需添加可播放节点。可量身定制 PlayableGraph 来适应当前情况的要求，而不是提供一个巨大的“一刀切”图形来启用和禁用节点。

---

* <span class="page-edit">2017-07-04  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">2017-07-04 Unity [2017.1](../Manual/30_search.html?q=newin20171) 中的新功能 <span class="search-words">NewIn20171</span></span>
