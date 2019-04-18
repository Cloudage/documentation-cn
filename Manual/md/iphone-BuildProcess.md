#iOS 构建过程详解

##基于 Editor 的构建和运行
iPhone/iPad 应用程序构建过程分为两步：

1.Unity 使用所有必需的库、预编译的 .NET 代码和序列化的资源生成 XCode 项目。
1.XCode 项目由 XCode 进行构建，并在实际设备上部署和运行。

在“Build settings”对话框中点击“Build”时，只完成第一步。点击“Build and Run”将执行这两个步骤。
如果用户在项目保存对话框中选择已存在的文件夹，则会显示警报。目前有两种 XCode 项目生成模式可供选择：

* __replace__ - 删除目标文件夹中的所有文件，并生成新内容
* __append__ - 清除“Data”、“Libraries”和项目根文件夹，并填充新生成的内容。根据最新的 Unity 项目更改对 XCode 项目文件进行更新。可将 XCode 项目“Classes”子文件夹视为放置自定义本机代码的安全位置，但仍建议进行定期备份。只有使用相同 Unity iOS 版本生成的现有 XCode 项目才支持 append 模式。

如果点击 Cmd+B，则会调用自动构建并运行过程，并将最新使用的文件夹作为构建目标。在这种情况下，默认采用 __append__ 模式。

注意：上面的第一步可在 PC 或 Mac 上执行。第二步只能在 Mac 上执行。这意味着，要在 iDevice 上运行 Unity 项目，必须拥有一台 Mac。

##命令行构建

一旦使用 Unity 来构建 XCode 项目，便可从命令行执行构建并运行。当 Editor 构建完 XCode 项目后，请从终端上执行以下命令：

````
unity$ xcodebuild test -destination "platform=iOS,id=400d20d00baf8d4997b47be0416cf5c44dd2d3bc" -scheme Unity-iPhone
````

请注意，上面命令行示例中的 400d20d00baf8d4997b47be0416cf5c44dd2d3bc 是要运行项目的 iDevice 的 ID。需使用 XCode 中的 Window > Devices 菜单确定设备 ID。
