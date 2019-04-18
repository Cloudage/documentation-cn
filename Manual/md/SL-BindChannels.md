#ShaderLab：旧版 BindChannels


__BindChannels__ 命令用于指定顶点数据映射到图形硬件的方式。

***注意：**使用[顶点程序](SL-ShaderPrograms.html)时，BindChannels 不起作用，因为在种情况下，绑定是由顶点着色器输入来控制的。建议现在使用可编程着色器，而不是固定函数顶点处理。*

默认情况下，Unity 会为您计算绑定，但在某些情况下，您可能希望使用自定义绑定。

例如，您可能会映射要在第一个纹理阶段中使用的主 UV 集，并映射要在第二个纹理阶段中使用的辅助 UV 集；或者告知硬件应将顶点颜色考虑在内。

语法
------

````
BindChannels { Bind "source", target }
````
指定顶点数据 _source_ 映射到硬件 _target_。

**Source** 可以是下列值之一：

* __Vertex__：顶点位置
* __Normal__：顶点法线
* __Tangent__：顶点切线
* __Texcoord__：主 UV 坐标
* __Texcoord1__：辅助 UV 坐标
* __Color__：每顶点颜色

**Target** 可以是下列值之一：

* __Vertex__：顶点位置
* __Normal__：顶点法线
* __Tangent__：顶点切线
* __Texcoord0__、__Texcoord1__ 等等：相应纹理阶段的纹理坐标
* __Texcoord__：所有纹理阶段的纹理坐标
* __Color__：顶点颜色

详细信息
-------


Unity 对于哪些源 (source) 可以映射到哪些目标 (target) 设置了一些限制。__Vertex__、__Normal__、__Tangent__ 和 __Color__ 的源和目标必须匹配。网格中的纹理坐标（__Texcoord__ 和 __Texcoord1__）可以映射到纹理坐标目标（对于所有纹理阶段来说，是 __Texcoord__；对于某个特定阶段来说，是 __TexcoordN__）。

BindChannels 有两个典型用例：

* 将顶点颜色考虑在内的着色器。
* 使用两个 UV 集的着色器。

示例
--------


````
// 将第一个 UV 集映射到第一个纹理阶段
// 并将第二个 UV 集映射到第二个纹理阶段
BindChannels {
   Bind "Vertex", vertex
   Bind "texcoord", texcoord0
   Bind "texcoord1", texcoord1
}
````


````
// 将第一个 UV 集映射到所有纹理阶段
// 并使用顶点颜色
BindChannels {
   Bind "Vertex", vertex
   Bind "texcoord", texcoord
   Bind "Color", color
}
````
