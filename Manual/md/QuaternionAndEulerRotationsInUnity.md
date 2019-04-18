#Unity 中的旋转和方向

##摘要
**3D 应用程序中的旋转通常以两种方式之一表示：四元数或欧拉角。每种方式都有自己的用途和缺点。Unity 在内部使用四元数表示，但在 Inspector 中显示等效的欧拉角值以便于进行编辑。**

## 欧拉角与四元数的区别

###欧拉角
欧拉角具有更简单的表示形式，即按顺序应用的 X、Y 和 Z 的三个角度值。要将欧拉旋转应用于特定对象，则依次应用每个旋转值，作为围绕其对应轴的旋转。

- **优点**：欧拉角具有直观的“可读”格式，由三个角度组成。
- **优点**：欧拉角可表示通过大于 180 度转向从一个方向到另一个方向的旋转
- **局限性**：欧拉角受到[万向锁 (Gimbal Lock)](https://en.wikipedia.org/wiki/Gimbal_lock) 的影响。当依次施加三个旋转时，第一个或第二个旋转可能导致第三个轴的方向与先前两个轴之一相同。这意味着已失去“自由度”，因为不能围绕唯一轴应用第三个旋转值。

###四元数
四元数可用于表示对象的方向或旋转。此表示方式在内部由四个数字组成（在 Unity 中称为 x、y、z 和 w），但是这些数字不代表角度或轴，您通常不需要直接访问它们。除非特别想深入研究[四元数的数学原理](https://en.wikipedia.org/wiki/Quaternions_and_spatial_rotation)，否则只需要知道四元数代表 3D 空间中的旋转即可，通常不需要知道或修改 x、y 和 z 属性。

与矢量可以表示位置或方向（从原点测量方向）的方式相同，四元数可以表示方向或旋转：从旋转“原点”或“[Identity](../ScriptReference/Quaternion-identity.html)”测量旋转。由于旋转的这种测量方式（从一个方向到另一个方向），因此四元数不能表示超过 180 度的旋转。

- **优点**：四元数旋转不受万向锁的影响。
- **局限性**：单个四元数不能表示任何方向超过 180 度的旋转。
- **局限性**：四元数的数字表示在直观上难以理解。

在 Unity 中，所有游戏对象旋转都在内部存储为四元数，因为这种表示方式的好处超过了局限性。

但是，在 Transform Inspector 中，我们使用欧拉角显示旋转，因为这种形式更容易理解和编辑。在 Inspector 中输入的用于游戏对象旋转的新值将在后台转换为该对象的新四元数旋转值。

![游戏对象的旋转在 Inspector 中以欧拉角的形式显示和编辑，但在内部存储为四元数](../uploads/Main/InspectorRotationEulerAngles.png)

此外还有一个副作用，在 Inspector 中可以输入游戏对象的旋转值，例如 **X：0，Y：365，Z：0**。但是，该值不能表示为四元数，所以点击 Play 时会看到该对象的旋转值变为 **X：0，Y：5，Z：0**（或近似的值）。这是因为旋转已被转换为四元数，而四元数没有“完整的 360 度旋转加 5 度”的概念，而是简单地设置为与旋转结果相同的方向。

##对脚本的影响

处理脚本中的旋转时，应使用 Quaternion 类及其函数来创建和修改旋转值。在某些情况下，使用欧拉角也是有效的，但应记住：
- 应使用处理欧拉角的 Quaternion 类函数
- 从旋转中检索、修改和重新应用欧拉值可能会导致意外的副作用。

### 直接创建和操作四元数

Unity 的 Quaternion 类具有许多函数可用于创建和操作旋转，而无需使用任何欧拉角。例如：

创建：

- [Quaternion.LookRotation](../ScriptReference/Quaternion.LookRotation.html)
- [Quaternion.AngleAxis](../ScriptReference/Quaternion.AngleAxis.html)
- [Quaternion.FromToRotation](../ScriptReference/Quaternion.FromToRotation.html)

操作：

- [Quaternion.Slerp](../ScriptReference/Quaternion.Slerp.html)
- [Quaternion.Inverse](../ScriptReference/Quaternion.Inverse.html)
- [Quaternion.RotateTowards](../ScriptReference/Quaternion.RotateTowards.html)
- [Transform.Rotate](../ScriptReference/Transform.Rotate.html) 和 [Transform.RotateAround](../ScriptReference/Transform.RotateAround.html)

但是，有时需要在脚本中使用欧拉角。在这种情况下，应注意必须将角度保存在变量中，并仅使用这些变量作为欧拉角*应用*于旋转。虽然可*从*四元数中检索欧拉角，但如果进行检索、修改和重新应用，则会出现问题。

下面是一些常犯**错误**的示例：使用假设的示例来尝试围绕 X 轴以每秒 10 度的速度旋转对象。应***避免***此类情况：

````
	// 旋转脚本错误 #1
		// 此处的错误在于我们正在修改四元数的 x 值
		// 此值不表示角度，不会产生所需的结果
	
		void Update () {
	
	        var rot = transform.rotation;
	        rot.x += Time.deltaTime * 10;
	        transform.rotation = rot;
		
		} 
````
	
````
	// 旋转脚本错误 #2
		// 此处的错误在于我们正在从四元数读取、修改再写入
		// 欧拉值。因为这些值是从四元数计算的，
		// 所以每个新的旋转可能会返回非常不同的欧拉角，而这可能会受到万向锁的影响。
		
		void Update () {
	    
	        var angles = transform.rotation.eulerAngles;
	        angles.x += Time.deltaTime * 10;
	        transform.rotation = Quaternion.Euler(angles);

		} 
````

以下是在脚本中***正确***使用欧拉角的示例：

````
	// 正确使用欧拉角的旋转脚本。
		// 此处我们将欧拉角存储在一个类变量中，并仅使用
		// 该变量作为欧拉角进行应用，但我们从不依赖于读回欧拉值。
		
	    float x;
		void Update () {
	    
	        x += Time.deltaTime * 10;
	        transform.rotation = Quaternion.Euler(x,0,0);

		}
````


##对动画的影响

许多 3D 制作包以及 Unity 自己的内部[动画窗口](AnimationEditorGuide.html)均允许使用欧拉角来指定动画期间的旋转。

这些旋转值通常可能超过四元数可表示的范围。例如，如果一个对象应当就地旋转 720 度，这可以用欧拉角“X：0，Y：720，Z：0”表示。但这不能通过四元数值来表示。

###Unity 的动画窗口
在 Unity 自己的动画窗口中，有一些选项允许指定如何进行旋转插值：使用四元数插值还是欧拉插值。通过指定欧拉插值，即告诉 Unity 您希望使用角度指定全范围运动。但是，使用四元数旋转，即表示仅希望旋转以特定方向结束，因此 Unity 将使用四元数插值并在最短距离内旋转到达该位置。请参阅[使用动画曲线](animeditor-AnimationCurves.html)以了解与此有关的更多信息。

###外部动画源
导入来自外部的动画时，这些文件通常包含欧拉格式的旋转关键帧动画。默认情况下，Unity 会重新采样这些动画，并为动画中的每个帧生成一个新的四元数关键帧，从而避免任何关键帧之间的旋转可能超过四元数的有效范围的情况。

例如，假设有两个关键帧，相隔 6 帧，X 的值在第一个关键帧上为 0，在第二个关键帧上为 270。如果不重新采样，这两个关键帧之间的四元数插值将在相反方向上旋转 90 度，因为这是从第一个方向到第二个方向的最短路径。但是，通过在每个帧上重新采样和添加关键帧，关键帧之间现在只有 45 度，因此旋转将正常工作。

不过，仍然存在一些情况，即使使用重新采样，导入动画的四元数表示可能还是与原来不够匹配，因此，在 Unity 5.3 及更高版本中，可选择关闭动画重新采样，这样就可以在运行时改用原始的欧拉动画关键帧。想了解更多相关信息，请参阅[欧拉曲线旋转的动画导入](AnimationEulerCurveImport.html)。
























