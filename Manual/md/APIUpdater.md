# 使用 Automatic API Updater

有时，在开发 Unity 软件期间，我们决定改变和改进类、函数和属性 (API) 的工作方式。我们在此过程中尽力对用户现有的游戏代码产生最小的影响，但有时为了力求完美，我们必须做出突破。

从 Unity 的一个重要版本过渡到另一个版本时，我们倾向于仅引入这些重要的“重大改变”，并且仅在可以提高 Unity 的易用性（意味着用户将产生更少的错误）或带来明显性能提升的情况下，而且仅在再三慎重考虑之后，才会做出这些改变。但是，这样做的结果是，如果在 Unity 5 中打开 Unity 4 项目，可能会发现使用的一些脚本命令现在已经被更改、删除或工作方式略有不同。

一个明显的例子是在 Unity 5 中删除了“快速访问器”，以前可以使用这种访问器直接引用游戏对象上的常见组件类型，例如 `gameObject.light`、`gameObject.camera`、`gameObject.audioSource` 等等。

在 Unity 5 中，现在必须对所有类型使用 GetComponent 命令（变换组件除外）。因此，如果在 Unity 5 中打开使用 `gameObject.light` 的 Unity 4 项目，可能会发现特定的代码行已*过时*而需要更新。

## 自动更新程序

Unity 包含一个 **Automatic Obsolete API Updater**，可检测脚本中是否使用了过时代码，并且会主动自动更新这些过时代码。如果接受更改，它将使用 API 的更新版本重新编写这些代码。

![API 更新对话框](../uploads/Main/APIUpdaterWarningDialog.png)

显然，与往常一样，为了防止出现问题，对工作内容备份非常重要，尤其是在允许该软件重新编写代码的情况下！一旦确定进行了备份，并单击了“Go Ahead”按钮，Unity 便会使用推荐的更新版本重新编写所有过时代码实例。

例如，如果有一个执行以下操作的脚本：

````
light.color = Color.red;
````

Unity 的 API Updater 会将其转换为：

````
GetComponent<Light>().color = Color.red;
````

更新程序的整体工作流程如下：

1.打开项目/导入包含使用了过时 API 的脚本/程序集的资源包
2.Unity 触发脚本编译
3.API Updater 检查是否存在它知道“可更新”的特定编译器错误
4.如果我们在上一步中发现任何编译错误，则会向用户显示对话框并提供自动更新，否则，我们已完成。
5.如果用户接受更新，则运行 API Updater（这样就会更新以步骤 2 中编译的相同语言所编写的所有脚本）
6.转到步骤 2（考虑所有更新的代码），直到在步骤 5 中没有脚本更新

因此，从上面的列表中可以看到，如果有脚本属于使用过时代码的不同编译过程（例如，不同语言的脚本、编辑器脚本等），则更新程序可能会多次运行。

API Updater 成功完成后，控制台中将显示通知，如下所示：

![成功！](../uploads/Main/APIUpdaterFinishedConsoleLog.png)

如果选择*不*允许 API Updater 更新脚本，在控制台中将像往常一样显示脚本错误。还可以看到，API Updater 可自动更新的错误在错误消息中标记为 **(UnityUpgradable)**。

![取消 API Updater 时在控制台中显示的错误](../uploads/Main/APIUpdaterRejectedConsoleErrors.png)

如果除了使用过时 API 之外，脚本还有其他错误，API Updater 可能无法完全完成其工作，除非修复了其他错误。在这种情况下，控制台窗口中将显示如下消息的通知：

![脚本中的其他错误可能会阻止 API Updater 正常工作。](../uploads/Main/APIUpdaterOtherErrors.png)

“Some scripts have compilation errors which may prevent obsolete API usages to get updated.Obsolete API updating will continue automatically after these errors get fixed.”

修复脚本中的其他错误后，可以再次运行 API Updater。触发脚本编译时，API Updater 会自动运行，也可以从 Assets 菜单中手动运行，位置如下：

![可以从 Assets 菜单中手动运行 API Updater。](../uploads/Main/APIUpdaterMenuOption.png)

## 命令行模式

从命令行中运行 Unity 时，可以使用 `-accept-apiupdate` 选项来运行 API Updater。请参阅[命令行参数](CommandLineArguments.html)以了解更多信息。


## 故障排除

如果收到消息“API Updating failed.Check previous console messages”，这意味着 API Updater 遇到了阻止其完成工作的问题。

导致这种情况的一个常见原因是，更新程序无法保存其更改，例如，用户无权修改更新的脚本。例如，该脚本可能具有写保护。

通过按照指示检查控制台中的前几行，应该能够看到更新过程中出现的问题。

![在此示例中，API Updater 失败，原因是其没有脚本文件的写权限。](../uploads/Main/APIUpdaterFailed.png)

## 限制

The API updater cannot automatically fix every API change. Generally, API that is upgradable is marked with `(UnityUpgradable)` in the obsolete message. For example:

```
[Obsolete(“Foo property has been deprecated. Please use Bar (UnityUpgradable)”)]
```

The API updater only handles APIs marked as `(UnityUpgradable)`. 

The API updater might not run if the only updatable API in your scripts include component or GameObject common properties, and those scripts only access members of those properties. Examples of common properties are `renderer` and `rigidbody`, and example members of those properties are `rigidbody.mass` and `renderer.bounds`. To workaround this, add a dummy method to any of your scripts to trigger the API updater. For example:

```
private object Dummy(GameObject o) { return o.rigidbody;}.
```

---
* <span class="page-edit">2018-06-02  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span><br/>
* <span class="page-history">Unity 2017.2 中添加了“accept-apiupdate”命令行选项</span>
