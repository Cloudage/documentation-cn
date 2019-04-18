#ShaderLab：SubShader

Unity 中的每个着色器都包含一个子着色器列表。当 Unity 必须显示网格时，它将找到要使用的着色器，并选择在用户的显卡上运行的第一个子着色器。


##语法

````
Subshader { [Tags] [CommonState] Passdef [Passdef ...]}
````

将子着色器定义为可选标签、通用状态和通道定义列表。


##详细信息

子着色器定义[渲染通道](SL-Pass.html)的列表，并且可选择性地设置所有通道共同的任意状态。此外，还可以设置子着色器专用的[标签](SL-SubShaderTags.html)。

当 Unity 选择要用于渲染的子着色器时，它会为每个定义的通道 (Pass) 渲染一次对象（并且数量可能由于光交互而增加）。由于对象的每次渲染成本都很高，因此应以尽可能少的通道数量定义着色器。当然，有时在某些图形硬件上，所需的效果不能在单个通道中完成；那么您别无选择，只能使用多个通道。

每个通道定义可以是[常规 Pass](SL-Pass.html)、[Use Pass](SL-UsePass.html) 或 [Grab Pass](SL-GrabPass.html)。

Pass 定义中允许的任何语句也可能出现在子 Subshader 代码块中。这将使所有通道都使用这一“共享”状态。


##示例


````
// ...
SubShader {
    Pass {
        Lighting Off
        SetTexture [_MainTex] {}
    }
}
// ...
````

以上子着色器定义了单个通道 (Pass)，用于关闭光照并且只显示一个名为 __\_MainTex__ 的纹理网格。
