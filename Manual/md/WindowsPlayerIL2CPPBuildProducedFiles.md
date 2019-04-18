# Windows 播放器：IL2CPP 构建文件

使用 IL2CPP 脚本后端的项目通常（由于构建设置不同可能会有所不同）生成以下文件：

![构建期间生成的 IL2CPP 文件](../uploads/Main/IL2CPPBuildProducedFiles.png)

以下文件是使用 Mono 或 IL2CPP 的项目共有的：

|**文件：** |**描述：** |
|:---|:---|
|__a_Data__| 包含游戏数据的文件夹。 |
|__a.exe__ | 主游戏可执行文件。 |
|__UnityCrashHandler64.exe__ | 崩溃处理程序可执行文件。 |
|__UnityPlayer.dll__ | 包含所有本机代码的 Unity Player 库。 |
|__WinPixEventRuntime.dll__ | PIX for Windows 运行时。此文件仅存在于开发版本中。 |

以下文件是使用 Mono 或 IL2CPP 的项目共有的：

|**文件：** |**描述：** |
|:---|:---|
|__a_BackUpThisFolder_ButDontShipItWithYourGame__| 包含调试游戏所需数据的文件夹，包括 PDB（调试信息）文件和脚本生成的 C++ 代码。应为发布的每个构建备份此文件夹，但不要重新分发它。 |
|__GameAssembly.dll__ | 包含 IL2CPP 运行时和所有脚本代码的库。 |
|__SymbolMap__ | 包含所有托管函数地址及其长度的列表的文件。IL2CPP 需要此文件来解析托管堆栈跟踪。如果将其删除，游戏仍将运行，但异常不会产生合理的调用堆栈

---
* <span class="page-edit">• 2018-03-13  Page published with [editorial review](DocumentationEditorialReview.html)
</span><br/>

* <span class="page-history">[2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 中的新功能 <span class="search-words">NewIn20181</span></span>
