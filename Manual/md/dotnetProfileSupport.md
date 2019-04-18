# .NET 配置文件支持

Unity 支持许多 .NET 配置文件。每个配置文件为 C# 代码提供不同的 API 表面来与 .NET 类库交互。可在 Player Settings（选择 __Edit__ > __Project Settings__ > __Player__）中更改 .NET 配置文件，为此需要使用 __Other Settings__ 部分的 __Api Compatibility Level__ 选项。

## 旧版脚本运行时

旧版脚本运行时支持两种不同的配置文件：.NET 2.0 Subset 和 .NET 2.0。这两者都与 Microsoft 的 .NET 2.0 配置文件密切相关。.NET 2.0 Subset 配置文件小于 .NET 4.x 配置文件，可用于访问大多数 Unity 项目使用的类库 API。此配置文件是大小受限平台（比如移动平台）的理想选择，并提供了一组可移植的 API 来实现多平台支持。默认情况下，大多数 Unity 项目应使用 .NET Standard 2.0 配置文件。

## 稳定脚本运行时

稳定脚本运行时支持两种不同的配置文件：.NET Standard 2.0 和 .NET 4.x。
.NET Standard 2.0 配置文件的名称可能有点误导，因为该配置文件与来自旧版脚本运行时的 .NET 2.0 和 .NET 2.0 Subset 配置文件无关。相反，Unity 支持的 .NET Standard 2.0 配置文件对应于 .NET Foundation 发布的同名配置文件。Unity 中的 .NET 4.x 配置文件对应于来自 .NET Framework 的 .NET 4 系列（.NET 4.5、.NET 4.6、.NET 4.7 等等）配置文件。

仅当需要确保与外部库的兼容性时，或者需要的功能在 .NET Standard 2.0 中不可用时，才应使用 .NET 4.x 配置文件。

## 跨平台兼容性

Unity 致力于在所有平台上支持 .NET Standard 2.0 配置文件中的绝大多数 API。虽然并非所有平台都完全支持 .NET Standard，但是旨在实现跨平台兼容性的库应指向 .NET Standard 2.0 配置文件。.NET 4.x 配置文件包含的 API 表面要大得多，包括可能在很少平台上运行甚至无法在任何平台上运行的部分。

## 托管插件
在 Unity 外部编译的托管代码插件可使用 Unity 中的 .NET Standard 2.0 配置文件或 .NET 4.x 配置文件。下表描述了 Unity 支持的配置：


||||API Compatibility Level:||
||||**.NET Standard 2.0** |**.NET 4.x** |
|:---|:---|:---|:---|
|Managed plugin compiled against:|||
|||__.NET Standard__|Supported| Supported |
|||__.NET Framework__ |Limited| Supported |
|||__.NET Core__ |Not Supported| Not Supported |

**注意**：

* 根据任何 .NET Standard 版本编译的托管插件均可用于 Unity。
* 有限支持表示，如果使用的来自 .NET Framework 的所有 API 都存在于 .NET Standard 2.0 配置文件中，则 Unity 支持该配置。但是，.NET Framework API 是 .NET Standard 2.0 配置文件的超集，因此有些 API 不可用。

---

* <span class="page-edit">2018-03-15  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 版中添加了 .NET 配置文件支持 <span class="search-words">NewIn20181</span></span>
