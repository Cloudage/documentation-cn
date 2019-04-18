将外部版本控制系统用于 Unity
=================================================


Unity 提供了 [Asset Server](AssetServer.html) 附加产品，从而轻松实现项目的集成化版本控制。此外，还可以使用 **Perforce** 和 **PlasticSCM** 外部工具（请参阅[版本控制集成](Versioncontrolintegration.html)以了解更多详细信息）。如果由于某种原因无法使用这些系统，可以将项目存储在其他任何版本控制系统（例如 Subversion 或 Bazaar）中。这需要对项目进行一些初始手动设置。

在签入项目之前，必须要求 Unity 稍微修改项目结构，使其符合在外部版本控制系统中存储资源的要求。要进行此操作，请在应用程序菜单中选择 __Edit &gt; Project Settings &gt; Editor__，然后在 Version Control 的下拉选单中选择 __Visible Meta Files__ 来启用外部版本控制支持。这将显示 `Assets` 目录中每个资源的文本文件，其中包含 Unity 所需的必要记录信息。这些文件将具有 `.meta` 文件扩展名，第一部分是与其关联的资源的完整文件名。在 Unity 中移动和重命名资源应该也会更新相关的 `.meta` 文件。但是，如果从外部工具移动或重命名资源，请确保也同步相关的 `.meta` 文件。

将项目签入版本控制系统时，应该将 `Assets`、`Packages` 和 `ProjectSettings` 目录添加到系统。应该完全忽略 `Library` 目录 - 使用 .meta 文件时，该目录只是导入资源的本地缓存位置。

创建新资源时，请确保将资源本身和关联的 `.meta` 文件都添加到版本控制中。

示例：创建新项目并将其导入到 Subversion 存储库中。
----------------------------------------------------------------------------


首先，假设我们在 ```svn://my.svn.server.com/``` 上有 Subversion 存储库且希望在 ```svn://my.svn.server.com/MyUnityProject``` 上创建项目。
然后按照以下步骤在系统中创建初始导入：


1.在 Unity 中创建新项目并将其命名为 `InitialUnityProject`。可以在其中添加任何初始资源，也可以稍后添加资源。
1.在 __Edit &gt; Project Settings &gt; Editor__ 中启用 __Visible Meta files__
1.退出 Unity（这可确保所有文件得到保存）。
1.删除项目目录内的 `Library` 目录。
1.将项目目录导入到 Subversion 中。如果使用的是命令行客户端，可以从初始项目所在的目录，按以下命令完成此操作：
```svn import -m"Initial project import" InitialUnityProject svn://my.svn.server.com/MyUnityProject```
如果成功，项目现在应该已经导入 Subversion，而且如果愿意，现在可以删除 `InitialUnityProject` 目录。
1.从 Subversion 签出此项目
```svn co svn://my.svn.server.com/MyUnityProject```，然后检查 `Assets`、`Packages` 和 `ProjectSettings` 目录是否已加入版本控制。
1.按住 __Option__ 或左侧 __Alt__ 键的同时启动 Unity，从而使用 Unity 来打开签出的项目。打开项目将重新创建以上步骤 4 中的 `Library` 目录。
1.**可选：**针对未加入版本控制的 `Library` 目录设置忽略过滤器：
```svn propedit svn:ignore MyUnityProject/``` 
Subversion 将打开文本编辑器。请添加 Library 目录。
1.最后，提交更改。项目现在应该已设置妥当并准备就绪：
```svn ci -m"Finishing project import" MyUnityProject```
