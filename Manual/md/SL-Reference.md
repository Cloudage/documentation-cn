# 着色器参考


Unity 中的着色器可以用三种不同的方式编写：


* 作为**[表面着色器](SL-SurfaceShaders.html)**，
* 作为**[顶点和片元着色器](SL-ShaderPrograms.html)**，或
* 作为**固定函数着色器**。

[着色器教程](Shaders.html)可以指导您选择适合您需求的类型。

无论您选择哪种类型的着色器，着色器代码的实际内容始终以名为 ShaderLab 的语言进行包装（这种语言用于组织着色器结构）。如下所示：



````
Shader "MyShader" {
    Properties {
        _MyTexture ("My Texture", 2D) = "white" { }
        // 此处还放置颜色或矢量等其他属性
    }
    SubShader {
        // 此处放置以下内容：
        // - 表面着色器或
顶点和片元着色器或
        // - 固定函数着色器
    }
    SubShader {
        // 此处放置比上面更简单的 SubShader 版本
        // 此版本能够在较旧的显卡上运行
    }
}
````

我们建议您首先阅读以下各部分了解关于 [ShaderLab 语法](SL-Shader.html)的一些基本概念，然后再继续阅读其他各部分了解表面着色器或顶点和片元着色器。由于固定函数着色器仅使用 ShaderLab 编写而成，因此您可以在 ShaderLab 参考本身中找到有关这些着色器的更多信息。

以下参考资料包含了不同类型着色器的大量示例。如果还需要特定表面着色器的更多示例，可从[资源部分 (Resources)](http://www.unity3d.com/support/resources/assets/built-in-shaders) 获取 Unity 内置着色器的源代码。Unity 的[后期处理效果](PostProcessingOverview.html)允许您使用着色器创建许多有趣的效果。

请继续阅读着色器参考资料并查看[着色器教程](Shaders.html)！


## 另请参阅

* [编写表面着色器](SL-SurfaceShaders.html)。
* [编写顶点和片元着色器](SL-ShaderPrograms.html)。
* [ShaderLab 语法参考](SL-Shader.html)。
* [着色器资源](class-Shader.html)。
* [高级 ShaderLab 主题](SL-AdvancedTopics.html)。
