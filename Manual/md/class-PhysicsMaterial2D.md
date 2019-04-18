2D 物理材质
===================


__2D 物理材质 (Physics Material 2D)__ 用于调整 2D 物理对象碰撞时这些对象之间的摩擦和弹性。可通过 Assets 菜单 (__Assets &gt; Create &gt; Physics Material 2D__) 创建 2D 物理材质。


![](../uploads/Main/PhysicsMaterial2DInspector.png) 


属性
----------



|**_属性：_** |**_功能：_** |
|:---|:---|
|__Friction__ |此碰撞体的摩擦系数。 |
|__Bounciness__ |碰撞从表面反弹的程度。值为 0 表示没有弹性，而值为 1 表示完美弹性，没有能量损失。 |


详细信息
-------

要使用 2D 物理材质，只需将其拖动到已附加 2D 碰撞体的对象上，或将其拖动到 Inspector 中的碰撞体组件。请注意，在 3D 物理中，等效资源称为 _Physic Material（物理材质）_（即 _physic_ 末尾没有 S）。在脚本中不要混淆这两种拼写，这很重要。
