#Smart Merge

Unity 包含一个名为 **UnityYAMLMerge** 的工具，能够以语义正确的方式合并场景和预制件文件。可从命令行中访问该工具，也可将其用于第三方版本控制软件。

##在 Unity 中设置智能合并

在 __Editor Settings__（菜单：__Edit &gt; Project Settings &gt; Editor__）中，可以选择第三方版本控制工具（例如 Perforce 或 PlasticSCM）。启用其中一个工具后，将在 _Version Control_ 标题下看到 _Smart Merge_ 菜单。该菜单具有四个选项：

* **Off**：仅使用偏好设置中设定的默认合并工具而不进行智能合并。
* **Premerge**：启用智能合并，接受纯净的合并。不纯净的合并将创建文件的预先合并基础版本、他们的版本和我的版本。然后，对这些版本使用默认合并工具。
* **Ask**：启用智能合并，但发生冲突时显示一个对话框让用户解决冲突（这是默认设置）。


##设置 UnityYAMLMerge 与第三方工具结合使用

UnityYAMLMerge 工具随附于 Unity Editor；假设 Unity 安装在标准位置，则 UnityYAMLMerge 的路径将是：

````
C:\Program Files\Unity\Editor\Data\Tools\UnityYAMLMerge.exe

or

C:\Program Files (x86)\Unity\Editor\Data\Tools\UnityYAMLMerge.exe
````

- 在 Windows 上；


````
/Applications/Unity/Unity.app/Contents/Tools/UnityYAMLMerge
````

- 在 Mac OSX 上（使用 Finder 中的 _Show Package Contents_ 命令访问此文件夹）。

UnityYAMLMerge 附带了一个默认的回退文件（称为 mergespecfile.txt，也在 Tools 文件夹中），用于指定如何处理未解决的冲突或未知文件。此外，还可将其用作不会根据文件扩展名自动选择合并工具的版本控制系统（例如 git）的主要合并工具。默认情况下，mergespecfile.txt 中已列出最常用的工具，但可以编辑此文件以添加新工具或更改选项。

可从命令行中将 UnityYAMLMerge 作为独立工具运行（若要查看该工具的完整使用说明，可在运行时不带任何参数）。下面给出了常见版本控制系统的设置说明。


###P4V
* 选择 Preferences &gt; Merge。
* 选择 _Other application_。
* 单击 _Add_ 按钮。
* 在 Extension 字段中，输入 `.unity`。
* 在 Application 字段中，输入 UnityYAMLMerge 工具的路径（见下文）。
* 在 Arguments 字段中，输入 `merge -p %b %1 %2 %r`
* 单击 Save。

然后，按照相同的步骤添加 `.prefab` 扩展名。


###Git
将以下文本添加到 `.git` 或 `.gitconfig` 文件中：

````
	[merge]
		tool = unityyamlmerge

		[mergetool "unityyamlmerge"]
		trustExitCode = false
		cmd = '<path to UnityYAMLMerge>' merge -p "$BASE" "$REMOTE" "$LOCAL" "$MERGED"
````

###Mercurial
将以下文本添加到 `.hgrc` 文件中：

````
	[merge-patterns]
		**.unity = unityyamlmerge
		**.prefab = unityyamlmerge

		[merge-tools]
		unityyamlmerge.executable = <path to UnityYAMLMerge>
		unityyamlmerge.args = merge -p --force $base $other $local $output
		unityyamlmerge.checkprompt = True
		unityyamlmerge.premerge = False
		unityyamlmerge.binary = False
````


###SVN
将以下文本添加到 `~/.subversion/config` 文件中：

````
	[helpers]
		merge-tool-cmd = <path to UnityYAMLMerge>
````


###TortoiseGit
* 选择 Preferences &gt; Diff Viewer &gt; Merge Tool，然后单击 _Advanced_ 按钮。
* 在弹出框的 Extension 字段中，输入 `.unity`。
* 在 External Program 字段中，输入：

````
	<path to UnityYAMLMerge> merge -p %base %theirs %mine %merged
````

然后，按照相同的步骤添加 `.prefab` 扩展名。


###PlasticSCM
* 选择 Preferences &gt; Merge Tools，然后单击 _Add_ 按钮。
* 选择 _External_ 合并工具。
* 选择 _Use with files that match the following pattern_。
* 添加 `.unity` 扩展名。
* 输入以下命令：

````
	<path to UnityYAMLMerge> merge -p "@basefile" "@sourcefile"  "@destinationfile" "@output"
````

然后，按照相同的步骤添加 `.prefab` 扩展名。


###SourceTree
* 选择 Tools &gt; Options &gt; Diff。
* 在 Merge Tool 下拉选单中选择 _Custom_。
* 在 _Merge Command_ 文本字段中，输入 UnityYAMLMerge 的路径。
* 在 _Arguments_ 文本字段中，输入 `merge -p $BASE $REMOTE $LOCAL $MERGED`。

