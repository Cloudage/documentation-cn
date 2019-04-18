# 关节和布娃娃稳定性

本页提供有关改善[关节](Joints.html)和[布娃娃](wizard-RagdollWizard.html)稳定性的技巧。

* 避免将 __Angular Y Limit__ 和 __Angular Z Limit__ 设置为较小的关节角度。根据具体设置，为了保持稳定，最小角度应在 5 到 15 度左右。尝试将角度设置为零，而不是使用小角度。这样会锁定轴并提供稳定的模拟。
* 取消选中关节的 __Enable Preprocessing__ 属性。通过禁用预处理，可在关节被强制进入无法满足关节约束条件的情况时帮助防止关节不规律地分离或移动。如果通过关节连接的[刚体](class-Rigidbody.html)组件被静态碰撞几何体拉开（例如，在墙内不完整生成布娃娃），则会发生这种情况。
* 在极端情况下（例如在墙内不完整生成布娃娃或用大力推动布娃娃），关节解算器无法将布娃娃的刚体组件保持在一起。这种情况下可能导致拉伸。要解决此问题，请使用 [ConfigurableJoint.projectionMode](../ScriptReference/ConfigurableJoint-projectionMode.html) 或 [CharacterJoint.enableProjection](../ScriptReference/CharacterJoint-enableProjection.html) 在关节上启用投影。
* 如果与关节连接的刚体组件抖动，请打开 Physics Manager (__Edit__ > __Project Settings__ > __Physics__) 并尝试将 __Default Solver Iterations__ 值增加到 10 到 20 之间。
* 如果与关节连接的刚体组件未准确响应反弹，请打开 Physics Manager (__Edit__ > __Project Settings__ > __Physics__) 并尝试将 __Default Solver Velocity Iterations__ 值增加到 10 到 20 之间。
* 在运动刚体组件由关节连接到其他刚体组件情况下，切勿使用直接变换访问。这样做会跳过 PhysX 计算相应刚体组件的内部速度的步骤，导致解算器提供意外的结果。一种常见的错误实践例子是在 2D 项目中使用直接变换访问通过在骨架的根节点上更改 [Transform.TransformDirection](../ScriptReference/Transform.TransformDirection.html) 来翻转角色。如果改用 [Rigidbody2D.MovePosition](../ScriptReference/Rigidbody2D.MovePosition.html) 和 [Rigidbody2D.MoveRotation](../ScriptReference/Rigidbody2D.MoveRotation.html)，那么此行为会改善很多。
* 避免关节连接的刚体组件之间的质量差异过大。一个刚体质量是另一个刚体质量的两倍是可以的，但当一个刚体质量比另一个刚体质量大十倍时，模拟就会变得不稳定。
* 尽量避免在包含刚体或关节的变换组件中使用不等于 1 的缩放。这样的缩放不可能在所有情况下都可靠。
* 如果刚体组件在插入到世界后发生重叠，并且无法避免重叠，请尝试降低 [Rigidbody.maxDepenetrationVelocity](../ScriptReference/Rigidbody-maxDepenetrationVelocity.html) 使刚体组件更加平滑地相互退出。
