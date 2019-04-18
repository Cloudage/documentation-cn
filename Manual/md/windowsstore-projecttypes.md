通用 Windows 平台：使用 .NET 脚本后端生成的项目
=============================
Unity 会生成 XAML C# Visual Studio 解决方案。生成 XAML C# 解决方案后，即可使用托管程序集，例如 UnityEngine.dll、Assembly-CSharp.dll 等，并在此基础上使用 XAML 代码。

Unity 将创建资源、vcproj、xaml 文件等文件。如果在同一目录之上构建项目，以下文件将**不会**被覆盖：

* 项目文件和解决方案文件（.vcproj、.sln 等）
* 源文件（App.xaml.cs、App.xaml.cpp）
* XAML 文件（App.xaml、MainPage.xaml 等）
* 图像文件（Assets\SmallTile.png、Assets\StoreLogo.png 等）
* 清单文件 - Package.appxmanifest

可安全地修改这些文件，如果要恢复到以前的状态，只需删除文件，然后在该文件夹的基础上构建项目。

**注意：**Unity 不会修改解决方案文件和项目文件（如果它们已存在于磁盘上）。Visual Studio 会采用整个 Data 文件夹而不是单个文件，因此如果有新文件添加到 Data 文件夹，则会自动选择该文件。

###配置

生成的 Visual Studio 项目有三种配置：**Debug**、**Release** 和 **Master**。**Debug** 配置具有各种安全检查，运行速度较慢；**Release** 配置会删除所有这些检查，但会启用性能分析器；**Master** 配置应当用于提交给应用商店的最终构建。

如果在左下角看到 **Development Build** 文本，则表示游戏不是为提交而构建的。如果从 Unity 进行构建时选择 **Development Build**，或者如果已构建 **Debug** 或 **Release** 配置，则会显示此文本。


---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
