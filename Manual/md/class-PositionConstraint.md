# 位置约束 (Position Constraints)

Position Constraint 组件移动游戏对象来跟随其源游戏对象。

![Position Constraint 组件](../uploads/Main/PositionConstraint.png)

## 属性

| **Property:** || **Function:** |
|:---|:---|
|__Activate__||After you position the constrained GameObject and its source GameObjects, click __Activate__ to save this information. __Activate__ saves the current offset from the source GameObjects in __Position At Rest__ and __Position Offset__, then checks __Is Active__ and __Lock__.|
|__Zero__||Sets the position of the constrained GameObject to the source GameObjects. __Zero__ resets the __Position At Rest__ and __Position Offset__ fields, then checks __Is Active__ and __Lock__.|
|__Is Active__||Toggles whether or not to evaluate the Constraint. To also apply the Constraint, make sure __Lock__ is checked.|
|__Weight__||The strength of the Constraint. A weight of 1 causes the Constraint to move this GameObject at the same rate as its source GameObjects. A weight of 0 removes the effect of the Constraint completely. This weight affects all source GameObjects. Each GameObject in the __Sources__ list also has a weight.|
|__Constraint Settings__|| |
||Lock|Toggle to let the Constraint move the GameObject. Uncheck this property to edit the position of this GameObject. You can also edit the __Position At Rest__ and __Position Offset__ properties. If __Is Active__ is checked, the Constraint updates the __At Rest__ or __Offset__ properties for you as you move the GameObject or its Source GameObjects. When you are satisfied with your changes, check __Lock__ to let the Constraint control this GameObject. This property has no effect in Play Mode.|
||Position At Rest|The X, Y, and Z values to use when __Weight__ is 0 or when the corresponding __Freeze Position Axes__ is not checked. To edit these fields, uncheck __Lock__.|
||Position Offset|The X, Y, and Z offset from the Transform that is imposed by the Constraint. To edit these fields, uncheck Lock.|
||Freeze Position Axes|Check X, Y, or Z to allow the Constraint to control the corresponding axes. Uncheck an axis to stop the Constraint from controlling it. This allows you to edit, animate, or script the unfrozen axis.|
|__Sources__||The list of GameObjects that constrain this GameObject. Each source has a weight from 0 to 1. |

---

* <span class="page-edit"> 2018-03-13  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 2018.1 版中添加了“约束”</span>
