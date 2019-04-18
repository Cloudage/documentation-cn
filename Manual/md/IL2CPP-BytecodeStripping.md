#使用 IL2CPP 进行托管字节码剥离
托管字节码剥离从托管程序集 (DLL) 中删除未使用的代码。该过程先定义根程序集，然后使用静态代码分析来确定这些根程序集使用的其他托管代码。任何无法访问的代码都将被删除。字节码剥离_不会_混淆处理代码，也不会以任何方式修改使用的代码。

对于给定的 Unity 播放器构建而言，根程序集是 Unity Editor 从脚本代码编译的程序集（例如，_Assembly-CSharp.dll_）。从脚本代码编译的任何程序集都不会被剥离，但其他程序集将被剥离。这些程序集包括：

* 您添加到项目的程序集
* Unity 引擎程序集
* .NET 类库程序集（如 mscorlib.dll、System.dll）

使用 [IL2CPP](IL2CPP.html) 脚本后端时，始终启用托管字节码剥离。在这种情况下，Stripping Level 选项将替换为名为 Strip Engine Code 的布尔值选项。如果启用此选项，则还将删除_本机_ Unity 引擎代码中未使用的模块和类。如果禁用此选项，则将保留本机 Unity 引擎代码中的所有模块和类。

可使用 _link.xml_ 文件（如下所述）通过保留类型和完整程序集来有效禁用字节码剥离。例如，要防止剥离系统程序集，可使用以下 link.xml 文件：

```
<linker>
       <assembly fullname="System" preserve="all"/>
</linker>
```

##提示
###使用反射时如何处理剥离
剥离在很大程度上取决于静态代码分析，有时无法有效完成，尤其是在使用反射等动态特性时。在这种情况下，有必要提供一些关于不应触及哪些类的提示。Unity 支持每个项目的自定义剥离黑名单。使用黑名单很简单，只需创建一个 link.xml 文件并将其放入 Assets 文件夹（或 Assets 的任何子目录）即可。下面提供了 link.xml 文件内容的示例。标记为保留状态的类不受剥离的影响：
 
```
<linker>
       <assembly fullname="System.Web.Services">
               <type fullname="System.Web.Services.Protocols.SoapTypeStubInfo" preserve="all"/>
       </assembly>

       <assembly fullname="System">
               <type fullname="System.Net.Configuration.WebRequestModuleHandler" preserve="all"/>
               <type fullname="System.Net.HttpRequestCreator" preserve="all"/>
               <type fullname="System.Net.FileWebRequestCreator" preserve="all"/>
       </assembly>

       <assembly fullname="mscorlib">
               <type fullname="System.AppDomain" preserve="fields"/>
               <type fullname="System.InvalidOperationException" preserve="fields">
                       <method signature="System.Void .ctor()"/>
               </type>
               <type fullname="System.Object" preserve="nothing">
                      <method name="Finalize"/>
               </type>
       </assembly>
</linker>
```

一个项目可以包含多个 link.xml 文件。每个 link.xml 文件可指定许多不同的选项。
assembly 元素指示应该应用嵌套指令的托管程序集。

type 元素用于指示应如何处理特定类型。它必须是 assembly 元素的子元素。fullname 属性可接受“*”通配符匹配一个或多个字符。

preserve 属性可以采用以下三个值之一：

* **all：**保留给定类型（或程序集，仅适用于 IL2CPP）的所有内容。
* **fields：**仅保留给定类型的字段。
* **nothing：**仅保留给定类型，但不保留其内容。

method 元素用于指示应保留特定方法。它必须是 type 元素的子元素。可按名称或签名来指定方法。

除了 link.xml 文件之外，还可以在源代码中使用 C# `[Preserve]` 属性来防止链接器剥离该代码。此属性的行为与 link.xml 文件中的相应条目略有不同：

* **Assembly：**保留程序集内的所有类型（就好像每个类型都有 `[Preserve]` 属性一样）
* **Type：**保留类型及其默认构造函数
* **Method：**保留方法的声明类型、返回类型及其所有参数的类型
* **Property、Field、Event：**保留属性、字段或事件的声明类型和返回类型

剥离的程序集将输出到项目中 Temp 目录下的目录（具体位置因目标平台而异）。原始的未剥离程序集在未剥离的目录中与剥离的程序集位于相同的位置。可使用 ILSpy 之类的工具来检查剥离的和未剥离的程序集，从而确定删除了代码的哪些部分。

---
* <span class="page-edit">2017-09-01 Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
 
* <span class="page-history">2017-05-26 - Unity 5.6 用户手册中进行了仅限于文档的更新</span>
* <span class="page-history">2017-09-01 - 为 Unity 2017.1 添加了关于使用 C# `[Preserve]` 属性的建议</span>
