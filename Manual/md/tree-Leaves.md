树叶组属性 (Leaf Group Properties)
=====================


树叶组将生成树叶几何体。基于基础类型或用户创建的网格。

Distribution
------------

调整树叶组中的树叶数量和位置。使用曲线可微调位置、旋转和缩放。这些曲线相对于父级树枝。


![](../uploads/Main/TreeNode-LeafNodePropertiesDistribution.png) 


| | |
|:---|:---|
|__Group Seed__ |此树叶组的种子。修改此设置可改变程序化生成过程。|
|__Frequency__ |调整为每个父级树枝创建的树叶数。|
|__Distribution__|选择树叶沿着父级分布的方式。|
|__Twirl__ |在父级树枝周围转动。|
|__Whorled Step__|定义使用轮生 (Whorled) 分布时每个轮生步骤中有多少个节点。对于真正的植物，这通常是斐波纳契数。|
|__Growth Scale__|定义沿着父节点生长的节点的比例。使用曲线进行调整并使用滑动条使效果淡入淡出。|
|__Growth Angle__|定义相对于父级的初始生长角度。使用曲线进行调整并使用滑动条使效果淡入淡出。|

Geometry
--------

选择为此树叶组生成的几何体类型以及应用的材质。如果使用自定义网格，则会使用其材质。


![](../uploads/Main/TreeNode-LeafNodePropertiesGeometry.png) 


| | |
|:---|:---|
|__Geometry Mode__|创建的几何体类型。您可以通过选择 Mesh 选项来使用自定义网格，适用于鲜花、水果等。|
|__Material__ |用于树叶的材质。|
|__Mesh__ |用于树叶的网格。|

Shape
-----

调整树叶的形状和生长。


![](../uploads/Main/TreeNode-LeafNodePropertiesShape.png) 


| | |
|:---|:---|
|__Size__ |调整树叶的大小，使用范围来调整最小和最大大小。|
|__Perpendicular Align__|调整树叶是否垂直于父级树枝对齐。|
|__Horizontal Align__ |调整树叶是否平行对齐。|

Animation
---------

调整用于动画化此树叶组的参数。风区仅在播放模式下有效。如果为 Main Wind 和 Main Turbulence 选择的值太高，树叶可能会从树枝上浮起。


![](../uploads/Main/TreeNode-LeafNodePropertiesAnimation.png) 


| | |
|:---|:---|
|__Main Wind__ |主风效果。通常情况下，此设置应保持为低值，以免树叶从父级树枝上浮起。|
|__Main Turbulence__|二级湍流效果。对于树叶，此设置通常应保持低值。|
|__Edge Turbulence__|定义沿树叶边缘发生的风湍流量。|
