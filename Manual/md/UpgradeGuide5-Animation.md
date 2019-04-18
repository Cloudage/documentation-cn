##Unity 5.0 中的动画功能

如果项目使用了动画功能，在将项目从 Unity 4 升级到 Unity 5 时需要注意以下注意事项。

###资源创建 API

在 5.0 中，我们引入了一个 API 用于在 Editor 中构建和编辑 Mecanim 资源。先前使用过不受支持的 API（在 UnityEditorInternal 命名空间中）的用户需要手动更新脚本以便使用这个新 API。

下面简要列出了最常见的类型更改：

|**_旧版：_** |**_新版：_** |
|:---|:---|:---|
|UnityEditorInternal.BlendTree |UnityEditor.Animations.BlendTree |
|UnityEditorInternal.AnimatorController |UnityEditor.Animations.AnimatorController |
|UnityEditorInternal.StateMachine |UnityEditor.Animations.AnimatorStateMachine |
|UnityEditorInternal.State |UnityEditor.Animations.AnimatorState |
|UnityEditorInternal.AnimatorControllerLayer |UnityEditor.Animations.AnimatorControllerLayer |
|UnityEditorInternal.AnimatorControllerParameter |UnityEditor.Animations.AnimatorControllerParameter |

另外请注意，大多数访问器函数已更改为数组：

````
UnityEditorInternal.AnimatorControllerLayer layer = animatorController.GetLayer(index);
````
变为：

````
UnityEditor.Animations.AnimatorControllerLayer layer = animatorController.layers[index];
````

 

以下博客文章末尾给出了 API 用法的基本示例：[http://blogs.unity3d.com/2014/06/26/shiny-new-animation-features-in-unity-5-0/](http://blogs.unity3d.com/2014/06/26/shiny-new-animation-features-in-unity-5-0/
)

有关更多详细信息，请参阅脚本 API 文档。
