# 通用 Windows 平台：.NET 脚本后端上缺少 .NET 类型

通用 Windows 应用程序上使用的 .NET 脚本后端是此平台的特殊 .NET 版本，与 Mono 不完全兼容。特别是缺少一些数据类型，而其他一些类没有某些方法，但 Mono 中相同的类具有这些方法。

为了使现有游戏更容易移植到通用 Windows 平台，Unity 提供了一些缺少的 .NET 类型。此外，还添加了一些扩展方法和替代类型，以简化迁移。这些类型位于 **PlaybackEngines\metrosupport\Managed\WinRTLegacy.dll** 中。

Unity 提供的类型包括：

* System.Collections.ArrayList
* System.Collections.Hashtable
* System.Collections.Queue
* System.Collections.SortedList
* System.Collections.Stack
* System.Collections.Specialized.HybridDictionary
* System.Collections.Specialized.ListDictionary
* System.Collections.Specialized.NameValueCollection
* System.Collections.Specialized.OrderedDictionary
* System.Collections.Specialized.StringCollection
* System.IO.Directory
* System.IO.File
* System.IO.FileStream
* System.Xml.XmlDocument
* System.Xml.XmlTextReader
* System.Xml.XmlTextWriter

除此之外，还添加了命名空间 WinRTLegacy 以提供更多类和扩展方法。其中包括：

* 大多数 System.IO 类的扩展方法 Close()（或者也可使用 Dispose()，此方法在适用于通用 Windows 平台的 Mono 和 .NET 上均可用）
* WinRTLegacy.TypeExtensions 具有 System.Type 的方法 GetConstructor()、GetMethod() 和 GetProperty()
* WinRTLegacy.IO.StreamReader 类，与 Mono System.IO.StreamReader 兼容
* WinRTLegacy.IO.StreamWriter 类，与 Mono System.IO.StreamWriter 兼容
* WinRTLegacy.Xml.XmlReader 类，与 Mono System.Xml.XmlReader 兼容
* WinRTLegacy.Xml.XmlWriter 类，与 Mono System.Xml.XmlWriter 兼容

如果命名空间不匹配，使用 WinRTLegacy 中的替代类的最简单方式是使用 using 指令：

```
# if NETFX_CORE
using XmlReader = WinRTLegacy.Xml.XmlReader;
# else
using XmlReader = System.Xml.XmlReader;
# endif
```

通过这种方式即可使用 XmlReader 类，此类将取自通用 Windows 平台上的 WinRTLegacy.Xml 命名空间以及其他地方的 System.Xml 命名空间。

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
