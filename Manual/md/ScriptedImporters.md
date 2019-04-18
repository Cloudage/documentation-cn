## 脚本化导入器 (Scripted Importer)

Scripted Importer 是 [Unity Scripting API](ScriptingSection.html) 的一部分。使用Scripted Importer在C# 中编写自定义资源导入器，用于Unity本身不支持的文件格式。

应通过专门定义抽象类 [ScriptedImporter](../ScriptReference/Experimental.AssetImporters.ScriptedImporter.html) 并应用 [ScriptedImporter](../ScriptReference/Experimental.AssetImporters.ScriptedImporterAttribute.html) 属性来创建自定义导入器。这样会注册您的自定义导入器以处理一个或多个文件扩展名。当资源管线检测到一个与注册的文件扩展名匹配的文件为新文件或已更改的文件时，Unity 会调用自定义导入器的 `OnImportAsset` 方法。

注意：Scripted Importer 无法处理已由 Unity 本身处理的文件扩展名。

**注意：限制**

这是 Scripted Importer 功能的实验性版本，因此仅限于可以使用 Unity Scripting API 创建的资源。这不是此功能在实现或设计方面的限制，而是对其实际使用施加了限制。


### 示例

下面是 Scripted Importer 的一个简单示例：它使用立方体图元作为主资源，将扩展名为“cube”的资源文件导入 Unity 预制件，并采用了从资源文件中获取的默认材质颜色：

```
using UnityEngine;
using UnityEditor.Experimental.AssetImporters;
using System.IO;

[ScriptedImporter(1, "cube")]
public class CubeImporter : ScriptedImporter
{
    public float m_Scale = 1;

    public override void OnImportAsset(AssetImportContext ctx)
    {
        var cube = GameObject.CreatePrimitive(PrimitiveType.Cube);
        var position = JsonUtility.FromJson<Vector3>(File.ReadAllText(ctx.assetPath));

        cube.transform.position = position;
        cube.transform.localScale = new Vector3(m_Scale, m_Scale, m_Scale);

        //“cube”为游戏对象，并将自动转换为预制件
        //（只有“主资源”才有资格成为预制件。）
        ctx.AddObjectToAsset("main obj", cube);
        ctx.SetMainObject(cube);

        var material = new Material(Shader.Find("Standard"));
        material.color = Color.red;

        // 必须为资源分配一个在所有导入之间保持一致的唯一标识符字符串
        ctx.AddObjectToAsset("my Material", material);

        // 必须销毁未作为导入输出传入上下文的资源
        var tempMesh = new Mesh();
        DestroyImmediate(tempMesh);
    }
}
```

**注意**：

* 导入器在Unity的资源管线中注册，是通过在CubeImporter类上放置`ScriptedImporter`属性。
* CubeImporter 类实现了抽象的 `ScriptedImporter` 基类。
* `OnImportAsset` 的 ctx 参数包含导入事件的输入和输出数据。
* 每个导入事件必须生成对 `SetMainAsset` 的一次（且仅一次）调用。
* 每个导入事件可以根据需要生成对 `AddSubAsset` 的任意次调用。
* 请参阅[脚本 API 文档](ScrptRef:Experimental.AssetImporters.ScriptedImporter.html)以了解更多详细信息。

您还可以实现自定义的导入设置编辑器 (Import Settings Editor)，为此需要专门定义 [ScriptedImporterEditor](../ScriptReference/Experimental.AssetImporters.ScriptedImporterEditor.html) 类，并使用 `CustomEditor` 类属性对其进行修饰以告知其用于哪种类型的导入器。

例如：


```
using UnityEditor;
using UnityEditor.Experimental.AssetImporters;
using UnityEditor.SceneManagement;
using UnityEngine;

[CustomEditor(typeof(CubeImporter))]
public class CubeImporterEditor: ScriptedImporterEditor
{
    public override void OnInspectorGUI()
    {
        var colorShift = new GUIContent("Color Shift");
        var prop = serializedObject.FindProperty("m_ColorShift");
        EditorGUILayout.PropertyField(prop, colorShift);
        base.ApplyRevertGUI();
    }
}
```

### 使用 Scripted Importer

将 Scripted Importer 类添加到项目后，可以像使用 Unity 支持的任何其他本机文件类型一样使用它：

* 将一个受支持文件拖放到资源目录层级视图中可导入该文件。
* 重新启动 Unity Editor 会重新导入自上次更新以来发生更改的所有文件。
* 在磁盘上编辑资源文件并返回 Unity Editor 会触发重新导入。
* 使用 __Asset__ > __Import New Asset...__ 导入新资源。
* 通过菜单 __Asset__ > __Reimport__ 显式触发重新导入。
* 单击资源可在 [Inspector 窗口](UsingTheInspector.html)中查看其设置。要修改其设置，请在 Inspector 窗口中编辑这些设置，然后单击 __Apply__。
<br/><br/>


![Scripted Importer 导入的资源 (An Alembic Girl) 的 Inspector 窗口](../uploads/Main/ScriptedImporters-2.png)


### Scripted Importer 的实际使用

* __Alembic__：Alembic Importer 插件已更新为使用 Scripted Importer。有关更多信息，请访问 [Unity github：AlembicImporter](https://github.com/unity3d-jp/AlembicImporter)。

* __USD__：USD Importer 插件已更新为使用 Scripted Importer。
有关更多信息，请访问 [Unity github：USDForUnity](https://github.com/unity3d-jp/USDForUnity)。

<br/>
<br/>

---

* <span class="page-history">[2017.1](https://docs.unity3d.com/2017.1/Documentation/Manual/30_search.html?q=newin20171) 中的新功能 <span class="search-words">NewIn20171</span></span>

* <span class="page-edit">2017-07-27  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span>


