通用 Windows 平台：性能分析器
=======================


可将 Unity 的性能分析器连接到正在运行的通用 Windows 应用程序。
执行以下步骤：

* 选择 **Player Settings** > **Publishing Settings** > **Capabilities**
* 启用专用网络（客户端和服务器）
* 从 Unity 构建通用 Windows 应用程序 Visual Studio 解决方案。
* 构建并运行应用程序
* 如果已选中 **Autoconnect Profiler** 复选框，性能分析器应该会自动连接到通用 Windows 应用程序，否则您必须在 Unity 中显式进行选择 (**Window** &gt; **Profiler** &gt; **Active Profiler**)，例如 MetroPlayerX86 (MyDevice)

**注意**：性能分析器不适用于 _Master_ 配置

**注意**：由于通用 Windows 平台限制，如果应用程序在同一台计算机上运行，您将无法连接性能分析器。例如，如果在同一台 PC 上运行 Unity Editor 和通用 Windows 平台，则无法连接。必须在一台计算机上运行 Unity Editor，在另一台计算机上运行通用 Windows 平台。此规则的唯一例外是“Autoconnect Profiler”构建选项，此选项会使应用程序连接到 Editor。

**注意**：还应确保运行 Unity Editor 的计算机和运行通用 Windows 应用程序的计算机**位于同一子网上**。

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
