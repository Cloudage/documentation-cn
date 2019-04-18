# ShaderLab：UsePass

UsePass 命令使用来自另一个着色器的指定通道。

## 语法

````
UsePass "Shader/Name"
````

插入来自给定着色器的具有给定名称的所有通道。_Shader/Name_ 包含着色器名称和通道名称，以斜杠字符分隔。请注意，系统只会考虑第一个受支持的[子着色器](SL-SubShader.html)。


## 详细信息


一些着色器可能会重复使用其他着色器中的现有通道，从而减少代码复制工作。例如，您可能有一个绘制对象轮廓的着色器通道，并希望在其他着色器中重复使用该通道。UsePass 命令就能实现您的愿望，该命令可包含来自其他着色器的指定通道。例如，以下命令使用来自内置 _VertexLit_ 着色器的名为“SHADOWCASTER”的通道：

````
UsePass "VertexLit/SHADOWCASTER"
````

要让 UsePass 运行，必须为要使用的通道提供一个名称。通道中的 [Name](SL-Name.html) 命令可为通道提供名称：

````
Name "MyPassName"
````

请注意：在内部，所有通道名称均大写，因此 UsePass 必须引用**大写**名称。
