#ShaderLab：Fallback

在所有子着色器的后面可定义 Fallback。基本上就是说，“如果没有任何子着色器能够在此硬件上运行，则尝试使用另一个着色器中的子着色器”。


##语法

````
	Fallback "name"
````
回退到具有给定_名称 (name)_ 的着色器，或者...


````
	Fallback Off
````
显式说明即使没有子着色器可以在此硬件上运行，也不会进行回退并且不会显示警告。


##详细信息

Fallback 语句的效果等同于插入其他着色器中的所有子着色器。


##示例

````
	Shader "example" {
		    // 此处为属性和子着色器...
		    Fallback "otherexample"
		}
````
