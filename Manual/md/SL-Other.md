# ShaderLab：其他命令


Category
--------


**Category** 是一种逻辑分组，其中包含属于该分组的所有命令。这主要用于“继承”渲染状态。例如，您的着色器可能有多个[子着色器](SL-SubShader.html)，并且其中每个子着色器都需要关闭[雾效](SL-Fog.html)、将[混合](SL-Blend.html)设置为附加等，便可为此使用 Category：


````
Shader "example" {
Category {
    Fog { Mode Off }
    Blend One One
    SubShader {
        // ...
    }
    SubShader {
        // ...
    }
    // ...
}
}
````

Category 代码块仅影响着色器解析，效果完全等同于将 Category 中设置的任意状态“粘贴”到 Category 下面的所有代码块中。这完全不会影响着色器执行速度。
