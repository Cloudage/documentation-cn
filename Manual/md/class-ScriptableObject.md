ScriptableObject
================


ScriptableObject 是一个可独立于脚本实例来存储大量共享数据的类。不要将此类与名称类似的 SerializedObject 混淆，后者是一个编辑器类，用于满足不同用途。例如，假设已经通过一个包含数百万整数的脚本生成了一个预制件。此数组占用 4MB 内存，并且属于预制件。每次实例化该预制件时，都将获得该数组的副本。如果创建了 10 个游戏对象，那么对于 10 个实例，最终将产生 40MB 的数组数据。

Unity 将所有原始类型、字符串、数组、列表、特定于 Unity 的类型（如 Vector3）以及具有可序列化属性的自定义类序列化为_副本_，这些副本属于声明了这些内容的对象。这意味着，如果创建了一个 ScriptableObject 并在其声明的数组中存储百万个整数，那么该数组将与该实例一起存储。这些实例被认为拥有自己的数据。ScriptableObject 字段或者任何 _UnityEngine.Object_ 字段（比如 MonoBehaviour、Mesh、GameObject 等等）按照_引用_来存储，而不是按照值来存储。如果某个脚本引用了包含百万个整数的 ScriptableObject，Unity 将只在脚本数据中存储对 ScriptableObject 的引用。ScriptableObject 继而存储该数组。引用 ScriptableObject 的预制件的 10 个实例（保存 4MB 数据）总共大约为 4MB 而不是另一个示例中所讨论的 40MB。

使用 ScriptableObject 的目标用途是通过避免值的副本来减少内存使用量，但也可以将其用于定义可插入的数据集。这方面的一个例子是，假设 RPG 游戏中有一个 NPC 商店。可以创建自定义 ShopContents ScriptableObject 的多个资源，每个资源定义一组可供购买的物品。在游戏具有三个区域的情况下，每个区域可以提供不同层级物品。商店脚本将引用一个 ShopContents 对象，而该对象定义可供购买的物品。请参阅脚本参考中的示例。

定义了 ScriptableObject 派生类后，可以使用 [CreateAssetMenu](../ScriptReference/CreateAssetMenuAttribute.html) 属性，从而轻松地使用该类创建自定义资源。

提示：在 Inspector 中使用 ScriptableObject 引用时，可以双击引用字段来打开 ScriptableObject 的 Inspector。还可以创建自定义编辑器来定义该类型的 Inspector 外观，从而帮助管理它所代表的数据。
