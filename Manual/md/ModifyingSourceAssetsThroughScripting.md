通过脚本修改源资源
=========================================


自动实例化
-----------------------

通常，想要对任何类型的游戏资源进行修改时，您会希望在运行时进行并希望是临时操作。例如，如果角色选择了无敌能量块，您可能想要更改玩家角色的__材质____着色器__以便直观展示无敌状态。此操作涉及修改正在使用的材质。此修改不是永久性的，因为当我们退出__播放模式__时，我们不希望材质具有不同的着色器。

但是，在 Unity 中可以编写永久修改源资源的脚本。让我们使用上面的材质示例作为起点。

要临时更改材质的着色器，我们可更改__材质__组件的__着色器__属性。

	private var invincibleShader = Shader.Find ("Specular");

	function StartInvincibility {
		renderer.material.shader = invincibleShader;
	}

使用此脚本并退出播放模式时，__[材质](../ScriptReference/Material.html)__的状态将重置为最初进入播放模式之前的状态。发生这种情况是因为无论何时访问 renderer.material，都会自动实例化材质并返回实例。此实例同时并自动应用于渲染器。因此，您可以进行真正想要的任何更改而不必担心永久性。


直接修改
-------------------


###重要注意事项
下面介绍的方法将修改 Unity 中使用的实际源资源文件。这些修改不可撤销。请谨慎使用。

现在，假如我们在退出播放模式时不希望材质重置。为此，可使用 [renderer.sharedMaterial](../ScriptReference/Renderer-sharedMaterial.html)。sharedMaterial 属性将返回此渲染器（以及其他渲染器）使用的实际资源。

下面的代码将永久更改材质来使用 Specular 着色器，不会将材质重置为进入播放模式之前的状态。

	private var invincibleShader = Shader.Find ("Specular");

	function StartInvincibility {
		renderer.sharedMaterial.shader = invincibleShader;
	}

如您所见，对 sharedMaterial 进行任何更改都可能在有用的同时又有风险。对 sharedMaterial 所做的任何更改都将是永久性的，而不是可撤销的。


适用的类成员
------------------------


上述相同的方法不仅可应用于材质。遵循该惯例的完整资源清单如下：


* 材质：renderer.material 和 renderer.sharedMaterial
* 网格：meshFilter.mesh 和 meshFilter.sharedMesh
* 物理材质：collider.material 和 collider.sharedMaterial


直接分配
-----------------


如果声明任何上述类（材质、网格或物理材质）的公共变量，并使用该变量而不是使用相关的类成员对资源进行修改，则不会获得在应用修改之前自动实例化的优点。

不自动实例化的资源
----------------------------------------------

有两种不同的资源在修改后绝不会自动实例化。

* [Texture2D](../ScriptReference/Texture2D.html)
* [TerrainData](../ScriptReference/TerrainData.html)

通过脚本对这些资源进行的任何修改都是永久性的，且永远不可撤销。因此，如果通过脚本更改地形的高度贴图，您需要考虑自己实例化和分配值。纹理也是如此。如果更改纹理文件的像素，则更改将是永久性的。

##iOS 和 Android 注意事项
在 iOS 和 Android 项目中修改 [Texture2D](../ScriptReference/Texture2D.html) 资源后绝不会自动实例化这些资源。通过脚本对这些资源进行的任何修改都是永久性的，且永远不可撤销。因此，如果更改纹理文件的像素，则更改将是永久性的。
