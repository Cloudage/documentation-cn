Perforce 集成
====================


有关 Perforce 的更多信息，请访问 [www.perforce.com](https://www.perforce.com/downloads/helix)。

设置 Perforce
-------------------


如果在[版本控制页面](Versioncontrolintegration.html)上遇到设置过程的任何问题，请参阅 [Perforce 文档](https://www.perforce.com/perforce/doc.current/manuals/p4v/)。

脱机使用 Perforce
-----------------------------


仅当您了解如何在没有沙盒的情况下脱机使用 Perforce，才应这样做。请参阅 [Perforce 文档](https://www.perforce.com/perforce/doc.current/manuals/p4v/using.offline.html)以了解更多信息。


故障排除
---------------


如果 Unity 由于某种原因无法将更改提交到 Perforce（例如服务器关闭、许可证问题等），这些更改将存储于单独的变更集。如果控制台未列出相关问题的任何信息，可使用 Perforce 的 P4V 客户端提交此变更集以查看具体的错误消息。

在提交时自动还原未更改的文件
---------------------------------------------


可配置 Perforce 在提交时还原未更改的文件，具体配置方法是在 P4V 中选择 **Connection > Edit Current Workspace...**，查看 Advanced 选项卡，然后将 On submit 的值设置为 **Revert unchanged files**：

![](../uploads/Main/VersionControl_P4V_RevertUnchangedFilesSetting.png) 
