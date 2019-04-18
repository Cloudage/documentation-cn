# 排除资源使其不发布到 Collaborate 

在某些情况下，您可能希望排除项目中的某些资源，使其不被发布到云端。Collaborate 使用 [gitignore](https://help.github.com/articles/ignoring-files/) 文件来排除不发布的文件。要排除不发布的资源，请将这些资源添加到 Project 根目录下的 _.collabignore_ 文件。此文件列出了在发布到 Collaborate 时需要排除的文件和文件夹。

向 _.collabignore_ 文件添加排除规则的步骤如下：

1.阅读 [Git-SCM.com 上的 GitIgnore 文档](https://git-scm.com/docs/gitignore)。
2.编辑 _.collabignore_ 文件以添加新规则。
3.启动 Unity Editor（如果 Editor 已经在运行，请将其重新启动）。
4.在 Collaborate 中发布您的 _.collabignore_ 文件更改，以便与团队的其他成员共享您的排除规则。

**注意**：为了使本地编辑的 *.collabignore* 文件生效，必须重新启动 Unity Editor。

**注意**：如果排除已在 Collaborate 中跟踪的文件，该文件现有的历史记录仍会保留。

有些项目文件和文件夹永远无法通过 *.collabignore* 文件从 Collaborate 中排除。如下：

* *.collabignore* 文件。
* [Assets](AssetWorkflow.html) 文件夹（但可以排除 *Assets* 文件夹中的特定文件或文件夹）。
* *Project Settings* 文件夹。
* *Project Settings* 文件夹中的任何 *.asset* 文件。
* *Project Settings* 文件夹中的 *ProjectVersion.txt*。

## 另请参阅

 [设置 Unity Collaborate](UnityCollaborateSettingUp.html)
