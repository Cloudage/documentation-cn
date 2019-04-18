在 Unity 4 中管理资源依赖关系
===========================

*在早于 Unity 5 的 Unity 版本中，必须单独使用编辑器脚本来定义资源依赖关系。（在 Unity 5 中，我们在编辑器中提供了工具，以方便将资源分配给特定的Bundle，并且依赖关系处理是自动进行的）。本页面的信息适用于在 Unity 4 中处理旧版项目的用户，并假设使用的是 Unity 4。*

Bundle 中的任何给定资源可能依赖于其他资源。例如，模型可能包含材质，而这些材质又会使用纹理和着色器。在资源的 Bundle 中可以附带该资源的所有依赖项。但是，来自不同 Bundle 的若干资源可能全部依赖于一组共同的其他资源（例如，若干不同的建筑物模型可以使用相同的砖块纹理）。如果一个共享依赖项的单独副本与使用该依赖项的对象一起包含在每个 Bundle 中，则在加载 Bundle 时将创建冗余的资源实例。这种情况将导致内存浪费。

为了避免这种浪费，可将共享的依赖项分离到一个单独的 Bundle 中，然后只需在需要这些依赖项的资源所在的 Bundle 中引用它们。首先，需要通过调用 [BuildPipeline.PushAssetDependencies](../ScriptReference/BuildPipeline.PushAssetDependencies.html) 来启用引用功能。然后，需要构建包含所引用依赖项的 Bundle。接下来，在构建引用第一个 Bundle 中的资源的 Bundle 之前，应再次调用 PushAssetDependencies。通过对 PushAssetDependencies 的进一步调用，可引入更深层次的依赖关系。引用级别存储在栈中，因此可以使用相应的 [BuildPipeline.PopAssetDependencies](../ScriptReference/BuildPipeline.PopAssetDependencies.html) 函数返回到某个级别。需要平衡 push 和 pop 调用，包括在构建之前发生的初始 push。

在运行时，必须先加载包含依赖项的 Bundle，然后再加载引用这些依赖项的任何其他 Bundle。例如，必须先加载一个包含共享纹理的 Bundle，然后再加载引用这些纹理的单独材质 Bundle。

资源 ID
---------


如果您预计需要重新构建作为依赖关系链一部分的 Asset Bundle，则应在启用 [BuildAssetBundleOptions.DeterministicAssetBundle](../ScriptReference/BuildAssetBundleOptions.DeterministicAssetBundle.html) 选项的情况下构建它们。这样可以保证用于标识资源的内部 ID 值在每次重新构建 Bundle 时都相同。

使用此方法构建 Asset Bundle 时，会为 Asset Bundle 中的对象分配一个 32 位哈希码；它是使用 Asset Bundle 文件的名称、资源的 GUID 以及资源内对象的本地 ID 进行计算得出的。因此，请确保在重新构建时使用相同的文件名。另外请注意，包含大量对象时可能会导致哈希冲突，从而阻止 Unity 构建 Asset Bundle。

着色器依赖关系
--------------------


每次将着色器作为参数在 [BuildPipeline.BuildAssetBundle](../ScriptReference/BuildPipeline.BuildAssetBundle.html) 中直接引用时，或通过选项 [BuildAssetBundleOptions.CollectDependencies](../ScriptReference/BuildAssetBundleOptions.CollectDependencies.html) 间接引用时，着色器的代码都将包含在 Asset Bundle 中。如果您单独使用 BuildAssetBundle 创建多个 Asset Bundle，这种情况可能会导致问题，因为引用的着色器将包含在每个生成的 Asset Bundle 中。这可能会存在冲突（例如您混合了着色器的不同版本时），因而不得不在修改着色器后重新构建所有 Bundle。此外，着色器的代码也会增加 Bundle 的大小。为了避免这些问题，可使用 [BuildPipeline.PushAssetDependencies](../ScriptReference/BuildPipeline.PushAssetDependencies.html) 将着色器分离到单个 Bundle 中，这样便可以仅更新着色器 Bundle。例如，为了实现此工作流程，可创建一个预制件以包含对所需着色器的引用：

###C# 示例


````
using UnityEngine;

public class ShadersList : MonoBehaviour {
	public Shader[] list;
} 


````

创建一个空对象，分配脚本，将着色器添加到列表中，并创建预制件（即“ShadersList”）。然后，您可以创建一个导出器，通过该导出器生成所有 Bundle 并更新着色器 Bundle：

###C# 示例


````
using UnityEngine;
using UnityEditor;

public class Exporter : MonoBehaviour {
	
	[MenuItem("Assets/Export all asset Bundles")]
	static void Export() {
		BuildAssetBundleOptions options = 
			BuildAssetBundleOptions.CollectDependencies | 
			BuildAssetBundleOptions.CompleteAssets | 
			BuildAssetBundleOptions.DeterministicAssetBundle;
		
		BuildPipeline.PushAssetDependencies();
		BuildPipeline.BuildAssetBundle(AssetDatabase.LoadMainAssetAtPath("Assets/ShadersList.prefab"), null, "Data/ShadersList.unity3d", options);
			
		BuildPipeline.PushAssetDependencies();
		BuildPipeline.BuildAssetBundle(AssetDatabase.LoadMainAssetAtPath("Assets/Scene1.prefab"), null, "Data/Scene1.unity3d", options);
		BuildPipeline.BuildAssetBundle(AssetDatabase.LoadMainAssetAtPath("Assets/Scene2.prefab"), null, "Data/Scene2.unity3d", options);		
		
		BuildPipeline.PopAssetDependencies();
		BuildPipeline.PopAssetDependencies();
	}
	
	[MenuItem("Assets/Update shader bundle")]
	static void ExportShaders() {
		BuildAssetBundleOptions options = 
			BuildAssetBundleOptions.CollectDependencies | 
			BuildAssetBundleOptions.CompleteAssets | 
			BuildAssetBundleOptions.DeterministicAssetBundle;
		
		BuildPipeline.PushAssetDependencies();
		BuildPipeline.BuildAssetBundle(AssetDatabase.LoadMainAssetAtPath("Assets/ShadersList.prefab"), null, "Data/ShadersList.unity3d", options);
		
		BuildPipeline.PopAssetDependencies();
	}
}


````

请记住，必须先加载着色器 Bundle。此方法有一个缺点：当对象数量太大时，选项 [BuildAssetBundleOptions.DeterministicAssetBundle](../ScriptReference/BuildAssetBundleOptions.DeterministicAssetBundle.html) 会因哈希冲突而产生冲突。在这种情况下，构建将失败，并且无法单独更新着色器 Bundle。此情况下必须删除该选项，然后重新构建所有 Asset Bundle。

