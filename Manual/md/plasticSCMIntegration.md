Plastic SCM 集成
=======================

有关 Plastic SCM 的更多信息，请访问他们的[网站](http://www.plasticscm.com)。

设置 Plastic SCM
----------------------


如果在[版本控制页面](Versioncontrolintegration.html)上遇到设置过程的任何问题，请参阅 [Plastic SCM 文档](http://www.plasticscm.com/infocenter.aspx)。

使用 Plastic SCM 签出文件
-----------------------------------

如果文件已修改，Plastic SCM 会自动签出文件，这样将更方便。唯一需要特定签出说明的文件是项目设置 (Project Settings) 文件，否则无法更改这些文件。

使用 Plastic SCM 解决冲突与合并
------------------------------------------------


在本地编辑项目中的某些文件时，如果远程也编辑了这些文件（冲突），则可能会发生合并。这意味着需要在执行合并之前查看更改。如果 Unity 认为必须在提交更改之前完成合并，则 Unity 会提示完成合并，这种情况下将跳转到 Plastic SCM 客户端。

如果传入的更改与本地更改冲突，则会在传入更改窗口中的冲突文件上显示一个问号图标。以下是使用 Plastic SCM 解决冲突与合并的快速指南：

* 在版本控制窗口中，单击“Apply all incoming changes”按钮，随即将自动转到 Plastic SCM GUI 客户端。
* 在客户端窗口中，可单击“Explain merge”以更直观的方式了解更改情况。现在单击“Process all merges”，随即将显示另一个窗口。
* 此处将显示各个冲突，并可以选择要保留或丢弃的更改。
* 解决冲突后，请单击保存并退出，这样就完成了合并操作。
* 现在需要通过 Unity 的版本控制窗口正常推送更改。

使用 Plastic SCM 锁定文件
------------------------------

为了使用 Plastic SCM 来锁定文件，需要遵循以下几个步骤：

* 首先必须创建一个 lock.conf 文件，并确保将该文件放在服务器目录中。可通过“../PlasticSCM/server”找到服务器目录。

* 在 lock.conf 文件中，必须指定正在处理的存储库以及要执行锁定检查的服务器。下面是一个示例：

````
rep:default lockserver:localhost:8087
*.unity
*.unity.meta
````
在此示例中，所有 .unity 和 .unity.meta 文件都将被锁定以便在存储库“default”上签出。

* 此时可能希望重新启动服务器，为此可打开终端/命令行窗口并查找服务器目录。进入目录后，可输入以下命令来重新启动服务器：

````
./plasticsd restart
````
* 现在返回 Unity 并签出希望锁定的文件，然后返回到终端/命令行并输入：

````
cm listlocks
````
如果已正确遵循这些步骤，则终端/命令行窗口现在应显示已锁定文件的列表。测试操作是否有效的另一种方法是尝试使用其他用户帐户签出同一文件，此时 Unity 的控制台中会显示错误，指出该文件已被其他用户签出。

有关更多信息，请访问 [Plastic SCM 锁定文件]( http://plasticscm.com/documentation/administration/plastic-scm-version-control-administrator-guide.shtml#C6_Checkout_Lock)文档。

使用 Plastic SCM 进行分布式和脱机工作
---------------------------------------------

如需进一步了解使用 Plastic SCM 在分布式模式 (DVCS) 和脱机状态下工作的更多信息，请查看[分布式版本控制指南 (Distributed Version Control Guide)](http://plasticscm.com/documentation/distributed/plastic-scm-version-control-distributed-guide.shtml)。
