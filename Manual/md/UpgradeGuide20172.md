#升级到 Unity 2017.2

<!-- Change submissions: https://docs.google.com/document/d/19UVEduEfbR6gjxQrl3Ob-0Wo4gGTilKUBU10lLpMNzM/edit --> 

本页面列出了从早期版本的 Unity 升级到 2017.2 版时可能对现有项目造成影响的更改。

例如：

* 数据格式的变化可能导致需要重新烘焙。

* 任何现有函数、参数或组件值的含义或行为的变化。

* 任何函数或功能的弃用。（建议的替代方案。）

***

**将 MonoBehaviour 添加到 Editor 中的游戏对象时，现在调用 MonoBehaviour.OnValidate**

在加载场景、复制游戏对象或 Inspector 中值发生变化时会调用 MonoBehaviour.OnValidate。现在，将 MonoBehaviour 添加到 Editor 中的游戏对象时，也会调用它。


***

**脚本：现在，在反序列化后会调用 InitializeOnLoad 回调**

InitializeOnLoad 的回调时间发生了变化。以前在调用 Unity API 时，调用 InitializeOnLoad 回调可能导致现有序列化对象出现无效的对象状态。现在，在反序列化之后以及创建所有对象之后调用该回调。在创建对象的过程中，必须调用默认构造函数。此更改意味着现在会在 InitializeOnLoad 静态构造函数之前调用对象构造函数，而以前是在某些对象构造函数之前调用 InitializeOnLoad。

示例：

```
[System.Serializable]
public class SomeClass
{
    public SomeClass()
    {
        Debug.Log("SomeClass constructor");
    }
}

public class SomeMonoBehaviour : MonoBehaviour
{
    public SomeClass SomeClass;
}

[InitializeOnLoad]
public class SomeStaticClass
{
    static SomeStaticClass()
    {
        Debug.Log("SomeStaticClass static constructor");
    }
}
```

以前此代码段的结果为：<br/>
`SomeStaticClass static constructor` (InitializeOnLoad)<br/>
`SomeClass constructor`（对象构造函数）

在进行此更改后，现在的结果为：以前此代码段的结果为：<br/>
`SomeClass constructor`（对象构造函数）<br/>
`SomeStaticClass static constructor` (InitializeOnLoad)


***


**新增了支持 BC5 格式的法线贴图类型。**

在此之前，Unity 支持不同压缩格式的 RGB 法线贴图或混合 AG 法线贴图（x 在 Alpha 通道，y 在绿色通道）。现在支持 RG 法线贴图（x 在红色通道，y 在绿色通道）。
UnpackNormal 着色器函数已升级，现在允许使用 RGB、AG 和 RG 法线贴图而无需添加着色器变体。为了能够做到这一点，UnpackNormal 函数依赖于将法线贴图的未使用通道设置为 1。即，一个混合 AG 法线贴图必须编码为 (1, y, 1, x)，而 RG 为 (x, y, 0, 1)。Unity 法线贴图编码器强制执行此设置。

如果用户使用的是未经修改的 Unity，则无需进行升级。然而，如果用户创建了自己的法线贴图着色器或自己的编码，他们可能需要考虑将混合 AG 法线贴图编码为 (1, y, 1, x)。如果用户在解压缩法线贴图之前在混合 AG 中混合了法线贴图，他们可能需要使用 UnpackNormalDXT5nm 而不是 UnpackNormal。



***




**在 Editor 中启动时始终预编译的托管程序集 (.dll) 和程序集定义文件程序集。**

即使在其他脚本中存在编译错误，也可以在 Editor 启动时加载预编译的托管程序集 (.dll) 和程序集定义文件程序集。这对于无论项目中是否存在其他脚本编译错误都应始终在启动时加载的 Editor 扩展程序集非常有用。

***


**HDR 发光。**

如果使用预计算实时 GI 或烘焙 GI，在早期版本的 Unity 中设置的强烈发光材质现在看起来会更加强烈，因为它们的范围不再受限制。先前使用的 RGBM 编码对于伽马空间给出了 97 的有效范围，对于线性颜色空间则给出了 8 的有效范围。HDR 拾色器的最大范围为 99，因此可以将某些材质设置为比看起来更强烈。
升级后，发光颜色作为真 HDR 16 位浮点值（范围现在为 64K）传递到 GI 系统。在内部，实时 GI 系统使用 rgb9e5 共享指数格式，这种格式可以表示这些强烈的值，但烘焙光照贴图受其 RGBM 编码的限制。烘焙光照贴图的 HDR 将在以后的版本中添加。

***


**VR 重命名为 XR。**

UnityEngine.VR.* 命名空间已重命名为 UnityEngine.XR.*。名称中包含 VR 的所有类型也已重命名为相应的 XR 版本。例如：UnityEngine.VR.VRSettings 现在变为 UnityEngine.XR.XRSettings，等等。

API Updater 已配置为自动将现有脚本和程序集更新为新类型名称和命名空间。如果不想使用 API Updater，还可以手动更新命名空间和类型。

命名空间更改：

* UnityEngine.VR -> UnityEngine.XR
* UnityEngine.VR.WSA -> UnityEngine.XR.WSA
* UnityEngine.VR.WSA.Input -> UnityEngine.XR.WSA.Input
* UnityEngine.VR.WSA.Persistence -> UnityEngine.XR.WSA.Persistence
* UnityEngine.VR.WSA.Sharing -> UnityEngine.XR.WSA.Sharing
* UnityEngine.VR.WSA.WebCam -> UnityEngine.XR.WSA.WebCam

UnityEngine.VR 类型更改：

* VRDevice -> XRDevice
* VRNodeState -> XRNodeState
* VRSettings -> XRSettings
* VRStats -> XRStats
* VRNode -> XRNode

所有 `VR.*` 性能分析器条目也已更改为 `XR.*`。

***

**UnityEngine.dll 现在针对每个 UnityEngine 模块拆分为独立的 dll。**

UnityEngine.dll（包含所有公共脚本 API）已被分成代码模块，涵盖引擎的不同子系统。这使 Unity 代码库更便于组织，具有更清晰的内部依赖关系，更适合使用内部工具，并使代码库更便于剥离。分离后的模块包括 UnityEngine.Collider（现在位于 UnityEngine.PhysicsModule.dll 中）和 UnityEngine.Font（现在位于 UnityEngine.TextRendering.dll 中）。

此更改通常应该不会影响任何现有项目，并且脚本现在会根据正确的程序集自动编译。Unity 现在包含一个 UnityEngine.dll 程序集文件，其中包含所有 UnityEngine 类型的[类型转发器](https://docs.microsoft.com/en-us/dotnet/framework/app-domains/type-forwarding-in-the-common-language-runtime)，适用于引用 DLL 的所有预编译程序集，通过将文件转发到新位置来确保向后兼容性。

但是，有一种情况，现有代码可能因为此更改而中断。也就是说，如果代码使用反射来获取 UnityEngine 类型，并假设所有类型都存在于同一个程序集内。这意味着此类代码将失败，因为 Collider 和 Font 现在位于不同的程序集内：

```
System.Type colliderType = typeof(Collider);
System.Type fontType = colliderType.Assembly.GetType("Font");
```

因为使用了类型转发器，从“UnityEngine”程序集获取 Collider 或 Font 类型仍然有效，如下所示：

```
System.Type.GetType("UnityEngine.Collider, UnityEngine")
```

Unity 仍然捆绑一个完整的单个 UnityEngine.dll，其中包含了 Unity Editor 的 Managed/UnityEngine.dll 文件夹中的所有 UnityEngine API。这样可以确保引用 UnityEngine.dll 的所有现有 Visual Studio/MonoDevelop 解决方案可以继续构建，而无需更新来引用新的模块化程序集。应继续使用此程序集在自定义解决方案中引用 UnityEngine API，因为模块的内部拆分可能会发生变化。

***

**标准着色器中的材质平滑度。**

使用 GGX 版本标准着色器的纯平滑材质现在可以接受镜面高光，因此增加了这些材质的真实感。



----

* <span class="page-edit">2017-10-06  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>
