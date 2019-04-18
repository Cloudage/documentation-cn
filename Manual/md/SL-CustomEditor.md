#ShaderLab：CustomEditor



可为着色器定义一个 CustomEditor。如果执行了此操作，Unity 将查找具有此名称并能扩展 ShaderGUI 的类。如果找到，则使用此着色器的所有材质都将使用此 ShaderGUI。有关示例，请参阅[自定义着色器 GUI](SL-CustomShaderGUI.html)。



##语法

````
	CustomEditor "name"
````
使用具有给定_名称 (name)_ 的 ShaderGUI。


##详细信息

CustomEditor 语句将影响使用此着色器的所有材质


##示例

````
Shader "example" {
    // 此处为属性和子着色器...
    CustomEditor "MyCustomEditor"
}
````
