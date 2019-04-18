Editor 故障排除
======================


下面的各个部分将介绍如何在不同情况下排除 Unity Editor 的故障以及预防问题。通常情况，应确保计算机满足所有[系统要求](http://unity3d.com/unity/system-requirements)，处于最新状态，并且您在系统中具有所需的用户权限。还要定期备份以确保项目的安全。

版本
--------


可以在不同的文件夹中安装不同版本的 Editor。但是，请务必备份项目，因为这些项目可能会被较新的版本升级，而无法在较旧版本的 Unity 中打开它们。请参阅关于[安装 Unity](InstallingUnity.html) 的手册页以了解更多信息。

附加组件的许可证仅对具有相同主要版本号（例如 3.x 和 4.x）的 Unity 版本有效。如果升级到 Unity 的次要版本，例如 4.0 升级到 4.1，则将保留附加组件。

激活
----------


互联网激活是生成 Unity 许可证的首选方法。但如果此过程出现问题，请按以下步骤操作：


1.断开计算机与网络的连接，否则可能会出现“tx_id invalid”错误。
1.选择 Manual Activation。
1.单击 Save License Request。
1.选择已知的保存位置，例如 Downloads 文件夹。
1.重新连接到网络，并打开 https://license.unity3d.com/
1.在 File 字段中，单击 Browse，然后选择许可证请求文件。
1.选择 Unity 所需的许可证，并填写所需的信息。
1.单击 Download License 并保存文件。
1.返回 Unity 并根据需要选择 Manual Activation。
1.单击 Read License，然后选择先前下载的许可证文件。

如果仍然无法注册或登录用户帐户，请联系 [support@unity3d.com](mailto:support@unity3d.com)。

无法启动
----------------


如果 Unity 在启动时崩溃，请首先要确保计算机符合最低[系统要求](http://unity3d.com/unity/system-requirements)。此外，还需要更新到最新的图形驱动程序和音频驱动程序。


如果出现磁盘写入错误，应检查用户帐户限制。在 MacOS 中时，请注意我们不建议使用“root 用户”，因为 Unity 尚未在此模式下进行测试。Unity 应始终对其文件夹具有写入权限，但如果选择手动授予这些权限，请检查以下文件夹：


Windows：

* Unity 的安装文件夹
* `%AllUsersProfile%\Unity`（通常为 C:\ProgramData\Unity）
* `C:\Documents and Settings\<user>\Local Settings\Application Data\Unity`
* `C:\Users\<user>\AppData\Local\Unity`


MacOS：

* Unity.app 包内容
* `/Library/Application Support/Unity`
* `~/Library/Logs/Unity`


使用以非本机分区方法格式化的硬盘以及使用某些软件在存储设备之间转移数据时，一些用户遇到过问题。


###字体

损坏的字体可能会导致 Unity 崩溃，可以按照以下步骤查找损坏的文件：


Windows：

1.打开计算机上的“Windows”文件夹中的 fonts 文件夹。
2.从“View”菜单中选择“Details”。
3.检查“Size”列中是否有大小为“0”的字体（表示文件存在问题）。
4.删除损坏的字体并重新安装。


MacOS：

1.启动 Font Book 应用程序。
2.选择所有字体。
3.打开“File”菜单并选择“Validate Fonts”，随后有问题的字体将显示为无效。
4.删除损坏的字体并重新安装。

系统可能资源受限，例如在虚拟机中运行的情况下。使用任务管理器查找占用大量内存的进程。


###损坏的项目或安装

Unity 可能会尝试打开一个损坏的项目，这可能包括默认的示例项目。在这种情况下，请重命名或移动项目的文件夹。Unity 正确启动后，可以根据需要恢复该项目的文件夹。

如果安装发生损坏，可能需要重新安装 Unity，请参阅下面的说明。

在 Windows 中，可能存在安装错误、注册表损坏、冲突等问题。例如，错误 0xC0000005 表示程序试图访问不应该访问的内存。如果最近添加了新硬件或驱动程序，请移除并更换硬件以确定是否是硬件导致的问题。运行诊断软件并查看关于操作系统故障排除的信息。


性能和崩溃
-----------------------


如果 Editor 运行缓慢或发生崩溃，特别是在构建时，这可能是由于正在消耗所有可用的系统资源引起的。请在构建项目时关闭所有其他应用程序。使用系统的实用程序对系统进行清理，并查看任务管理器 (Windows) 或活动监视器 (MacOS) 确定是否存在使用大量资源（例如内存）的进程。有时，病毒防护软件可能因为扫描过程而减慢甚至阻止文件系统。

项目丢失
------------


有许多因素可能导致项目损坏，因此应**定期备份项目**，以防发生不幸的事故。在 MacOS 中，请使用专用的外部硬盘驱动器激活 TimeMachine。项目丢失后，可以尝试使用任何文件恢复实用程序进行恢复，但有时是不可逆转的。


重新安装
---------------


请按照以下步骤重新安装 Editor：


1.卸载 Unity。在 MacOS 中，将 Unity 应用程序拖到垃圾箱。


1.删除以下文件（如果存在）：


    * Windows：
        * `%AllUsersProfile%\Unity\`（通常为 C:\ProgramData\Unity）


    * MacOS：
        * `/Library/Application Support/Unity/`


1.重新启动计算机。


1.由于原始安装可能已损坏，请从我们的网站下载最新版本：http://unity3d.com/unity/download/archive


1.重新安装 Unity。
