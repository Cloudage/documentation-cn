自定义着色器 GUI
=======================


有时着色器中会包含一些有趣的数据类型，而使用内置的 Unity 材质编辑器无法很好地表示这些数据类型。Unity 提供了一种方法来覆盖显示着色器属性的默认方式，以便您可以定义自己的方式。您可以使用此功能来定义自定义控件和数据范围验证。

要为着色器的 GUI 编写自定义编辑器，首先要定义一个需要[自定义编辑器](SL-CustomEditor.html)的着色器。用于自定义编辑器的名称是 Unity 将为材质编辑器查找的类。

要定义自定义编辑器，请展开 ShaderGUI 类，并将脚本放在 Assets 目录中的 Editor 文件夹下。



````
using UnityEditor;

public class CustomShaderGUI : ShaderGUI 
{
	public override void OnGUI (MaterialEditor materialEditor, MaterialProperty[] properties)
	{
		base.OnGUI (materialEditor, properties);
	}
} 
````

任何定义了自定义编辑器 (**CustomEditor "CustomShaderGUI"**) 的着色器都将实例化以上列出的着色器 GUI 类，并执行相关联的代码。

一个简单示例
--------------

我们现在的情况是，有一个着色器能以两种模式工作；该着色器渲染标准漫射光照，或者渲染 50% 的蓝色和绿色通道。



````
Shader "Custom/Redify" {
	Properties {
		_MainTex ("Base (RGB)", 2D) = "white" {}
	}
	SubShader {
		Tags { "RenderType"="Opaque" }
		LOD 200
		
		CGPROGRAM
		#pragma surface surf Lambert addshadow
		#pragma shader_feature REDIFY_ON

		sampler2D _MainTex;

		struct Input {
			float2 uv_MainTex;
		};

		void surf (Input IN, inout SurfaceOutput o) {
			half4 c = tex2D (_MainTex, IN.uv_MainTex);
			o.Albedo = c.rgb;
			o.Alpha = c.a;

			#if REDIFY_ON
			o.Albedo.gb *= 0.5;
			#endif
		}
		ENDCG
	} 
	CustomEditor "CustomShaderGUI"
}
````

如您所见，着色器有一个可以设置的关键字：REDIFY_ON。可使用材质的 shaderKeywords 属性按照材质来设置此关键字。以下是执行此操作的 ShaderGUI 实例。


````
using UnityEngine;
using UnityEditor;
using System;

public class CustomShaderGUI : ShaderGUI
{
	public override void OnGUI(MaterialEditor materialEditor, MaterialProperty[] properties)
	{
		// 渲染默认 GUI
		base.OnGUI(materialEditor, properties);

		Material targetMat = materialEditor.target as Material;

		// 检查是否设置了 redify 并显示一个复选框
		bool redify = Array.IndexOf(targetMat.shaderKeywords, "REDIFY_ON") != -1;
		EditorGUI.BeginChangeCheck();
		redify = EditorGUILayout.Toggle("Redify material", redify);
		if (EditorGUI.EndChangeCheck())
		{
			// 根据复选框来启用或禁用关键字
			if (redify)
				targetMat.EnableKeyword("REDIFY_ON");
			else
				targetMat.DisableKeyword("REDIFY_ON");
		}
	}
}
````

有关更全面的 ShaderGUI 示例，请参阅位于“内置着色器”软件包中的 StandardShaderGUI.cs 文件以及 Standard.shader（可从 [Unity 下载存档 (Unity Download Archive)](http://unity3d.com/unity/download/archive) 下载该软件包）。

请注意，上面这个简单示例还可以使用 MaterialPropertyDrawers 来更加简便地解决。向 Custom/Redify 着色器的 Properties 部分中添加以下行：

````
[Toggle(REDIFY_ON)] _Redify("Red?", Int) = 0
````

并删除：

````
CustomEditor "CustomShaderGUI"
````

另请参阅：[MaterialPropertyDrawer](../ScriptReference/MaterialPropertyDrawer.html)


对于更复杂的着色器 GUI 解决方案（例如，材质属性彼此依赖或者希望有特殊布局），则应该使用 ShaderGUI。您可以将 MaterialPropertyDrawers 与 ShaderGUI 类组合使用，请参阅 StandardShaderGUI.cs。
