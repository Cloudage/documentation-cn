2D 区域效应器 (Area Effector 2D)
=========

当目标 2D 碰撞体与 2D 区域效应器接触时，2D 效应器会在附加的 2D 碰撞体所定义的区域内施力。您可以在任何角度为此力配置特定幅度以及该幅度内的随机变化。还可以应用线性阻力和角阻力来减慢 2D 刚体的速度。

与 2D 区域效应器结合使用的 2D 碰撞体通常会设置为触发器，这样其他 2D 碰撞体就能与其重叠，从而施力。非触发器仍然有效，但只有 2D 碰撞体与其接触时才会施力。


![Area Effector 2D Inspector](../uploads/Main/AreaEffector2DInspector.png)

属性
----------



|**_属性：_** |**_功能：_** |
|:---|:---|
|__Use Collider Mask__ |选中此选项可启用 __Collider Mask__ 属性。如果未启用此选项，则所有 2D 碰撞体都将默认采用全局碰撞矩阵 (Global Collision Matrix)。|
|__Collider Mask__ |此遮罩用于选择允许与 2D 区域效应器进行交互的特定层。 |
|__Use Global Angle__ |选中此选项可将 __Force Angle__ 定义为全局（世界空间）角度。如果未选中，物理引擎会将 __Force Angle__ 视为局部角度。 |
|__Force Angle__ |要施加的力的角度。 |
|__Force Magnitude__ |要施加的力的大小。 |
|__Force Variation__ |要施加的力的大小变化。 |
|__Drag__ |应用于 2D 刚体的线性阻力。 |
|__Angular Drag__ |应用于 2D 刚体的角阻力。 |
|__Force Target__ |2D 区域效应器在目标游戏对象上施力的作用点。|
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;Collider|目标点定义为 2D 碰撞体的当前位置。如果 2D 碰撞体没有位于质心处，则在此处施力会产生扭矩（旋转）。|
|&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;&amp;#160;Rigidbody|目标点定义为 2D 刚体的当前质心。在此处施力绝对不会产生扭矩（旋转）。 |

