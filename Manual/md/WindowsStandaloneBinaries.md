#Windows 独立平台播放器构建二进制文件
以 Windows 独立平台为目标构建 Unity 项目时，Unity 会生成以下文件（其中“ProjectName”为项目名称）：

* `ProjectName.exe` - 项目可执行文件。此文件包含启动时调用 Unity 引擎的程序入口点。

* `UnityPlayer.dll` - 此 DLL 包含所有本机 Unity 引擎代码。还会使用 Unity Technologies 证书对此文件进行签名，从而轻松验证引擎未被篡改。

* `*.pdb files` - 用于调试的符号文件。如果在 Build Settings 窗口中启用了 __Copy PDB files__，Unity 会将这些文件复制到构建目录。

* `WinPixEventRuntime.dll` - 此 DLL 可启用 [Windows PIX 性能分析器](https://blogs.msdn.microsoft.com/pix/2017/01/17/introducing-pix-on-windows-beta/)支持。仅当在 Build Settings 窗口中选中 __Development Build__ 复选框时，Unity 才会创建此文件。

* `ProjectName_Data folder` - 此文件夹包含运行项目所需的所有数据。

##重新构建游戏可执行文件
Unity 在以下文件夹中创建 `ProjectName.exe` 的源代码：*Editor\Data\PlaybackEngines\WindowsStandaloneSupport\Source\WindowsPlayer*。

要修改可执行文件或发布您自己构建的代码（例如，如果要对其进行签名），必须重新构建可执行文件并将其放在构建的游戏目录中。

要在 Unity 外部构建可执行文件，必须具备 [Visual Studio](https://www.visualstudio.com/) 2015 或 2017 并安装“Common Tools for Visual C++”和“Windows XP support for C++”。

---
* <span class="page-edit">2017-07-19  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2017.2](https://docs.unity3d.com/2017.2/Documentation/Manual/30_search.html?q=newin20172) 版中更改了 Windows 独立平台播放器构建二进制文件 <span class="search-words">NewIn20172</span></span>

