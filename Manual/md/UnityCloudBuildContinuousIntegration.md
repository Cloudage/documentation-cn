# Automated Build Generation

Automated Build Generation 是 Cloud Build for Unity Teams Advanced 的一项功能。只要有团队成员更改项目，该功能就会通过自动发布变更并编译项目来提供持续集成。团队成员提交更改时，Cloud Build 会使用共享代码仓库中的最新代码来编译项目。

Automated Build Generation 的目标是在项目每次发生更改时对项目进行编译。因此可以提供最准确的错误发生时间和位置信息，并确保您始终基于最后正常的提交结果来编译游戏。

## Automated Build Generation 工作原理

Unity Cloud Build 连接到 [Unity Collaborate](UnityCollaborate.html) 或源代码控制系统，并通过该系统了解项目是否发生了更改。Unity Cloud Build 检测到项目发生更改时，会下载项目并针对所有目标平台编译项目。编译完成后，Unity Cloud Build 会通过电子邮件向您发送结果并附带用于下载和安装这些编译版本的链接。如果出现错误，Unity Cloud Build 会立即通知您，让您快速修复错误、提交更改并触发新编译。


-----
<span class="page-edit">2018-04-10  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>
