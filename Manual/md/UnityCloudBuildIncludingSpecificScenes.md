# 包含特定场景

默认情况下，Unity Cloud Build 会编译已在 Unity Editor 里通过 __File__ > __Build Settings__ > __Scenes in Build__ 添加到项目中的场景。单击 __Add Open Scenes__ 可添加更多场景。请参阅有关[编译发布 (Publishing builds)](PublishingBuilds.html) 的文档以了解更多信息。

若要指示 Unity Cloud Build 忽略 Unity Editor 中的 __Scenes in Build__ 列表而编译其他场景列表，您需要通过 [Unity 开发者网站](https://developer.cloud.unity3d.com)提供__场景列表__。

在 Unity 开发者网站上，转至编译目标的 __Advanced Options__。（请参阅有关访问和编辑 [Advanced Options](UnityCloudBuildAdvancedOptions.html) 的文档。）



![Edit Advanced Options 屏幕](../uploads/Main/UnityCloudBuildAdvancedOptions-AdvancedOptionsEdit.png)

将想要包含的场景列表添加到 __Scene List__。对于希望包含的每个场景文件，以从 __Assets__ 目录起的相对路径提供文件路径。

这对于开发很有用。例如，您可能有一个编译目标包含了所有场景，但需要很长时间才能完成编译。如果您只使用一个场景，可能会发现创建另一个仅包含这一个场景的编译目标会很有用。此编译过程应该会快得多，并且允许更快迭代开发版本。
