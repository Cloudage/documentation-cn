# 缩放约束 (Scale Constraints)

Scale Constraint 组件可调整游戏对象的大小来匹配其源游戏对象的比例。

![Scale Constraint 组件](../uploads/Main/ScaleConstraint.png)

### 属性

| **Property:** || **Function:** |
|:---|:---|
|__Activate__||After you resize the constrained GameObject and its source GameObjects, click __Activate__ to save this information. __Activate__ saves the current offset from the source GameObjects in __Scale At Rest__ and __Scale Offset__ then checks __Is Active__ and __Lock__.|
|__Zero__||Sets the rotation of the constrained GameObject to the source GameObjects. __Zero__ resets the __Scale At Rest__ and __Scale Offset__ fields then checks __Is Active__ and __Lock__.|
|__Is Active__||Toggles whether or not to evaluate the Constraint. To also apply the Constraint, make sure __Lock__ is checked.|
|__Weight__||The strength of the Constraint. A weight of 1 causes the Constraint to resize this GameObject at the same rate as its source GameObjects. A weight of 0 removes the effect of the Constraint completely. This weight affects all source GameObjects. Each GameObject in the __Sources__ list also has a weight.|
|__Constraint Settings__||&nbsp;|
||Lock|Toggle to let the Constraint resize the GameObject. Uncheck this property to edit the scale of this GameObject. You can also edit the __Scale At Rest__ and __Scale Offset__ properties. If __Is Active__ is checked, the Constraint updates the __Scale At Rest__ or __Scale Offset__ properties for you as you resize the GameObject or its Source GameObjects. When you are satisfied with your changes, check __Lock__ to let the Constraint to control this GameObject. This property has no effect in Play Mode.|
||Scale At Rest|The X, Y, and Z values to use when __Weight__ is 0 or when the corresponding __Freeze Scale Axes__ is not checked. To edit these fields, uncheck __Lock__.|
||Scale Offset|The X, Y, and Z offset from the transform that is imposed by the Constraint. To edit these fields, uncheck Lock.|
||Freeze Scale Axes|Check X, Y, or Z to allow the Constraint to control the corresponding axes. Uncheck an axis to stop the Constraint from controlling it. This allows you to edit, animate, or script the unfrozen axis.|
|__Sources__||The list of GameObjects that constrain this GameObject. Each source has a weight from 0 to 1. |

---

* <span class="page-edit"> 2018-03-13  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 2018.1 版中添加了“约束”</span>
