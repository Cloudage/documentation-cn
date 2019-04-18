# 向顶点程序提供顶点数据

对于 Cg/HLSL [顶点程序](SL-ShaderPrograms.html)，
[网格](class-Mesh.html)顶点数据作为输入传递给顶点
着色器函数。每个输入都需要有指定的[语义](SL-ShaderSemantics.html)：例如，`POSITION` 输入表示顶点位置，`NORMAL` 表示顶点法线。

通常，顶点数据输入在结构中声明，而不是
逐个列出。在 [UnityCG.cginc include 文件](SL-BuiltinIncludes.html)中
定义了几个常用的顶点结构，在大多数情况下，
仅使用它们就足够了。这些结构为：

* `appdata_base`：位置、法线和一个纹理坐标。
* `appdata_tan`：位置、切线、法线和一个纹理坐标。
* `appdata_full`：位置、切线、法线、四个纹理坐标和颜色。

**示例：**以下着色器根据法线为网格着色，并使用 `appdata_base` 作为顶点程序输入：


````
Shader "VertexInputSimple" {
	SubShader {
		Pass {
			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			#include "UnityCG.cginc"
		 
			struct v2f {
				float4 pos : SV_POSITION;
				fixed4 color : COLOR;
			};
			
			v2f vert (appdata_base v)
			{
				v2f o;
				o.pos = UnityObjectToClipPos(v.vertex);
				o.color.xyz = v.normal * 0.5 + 0.5;
				o.color.w = 1.0;
				return o;
			}

			fixed4 frag (v2f i) : SV_Target { return i.color; }
			ENDCG
		}
	} 
}
````

要访问不同的顶点数据，您需要自己声明
顶点结构，或者将输入参数添加到
顶点着色器。顶点数据由 Cg/HLSL
[语义](SL-ShaderSemantics.html)标识，并且必须来自
以下列表：

* `POSITION` 是顶点位置，通常为 `float3` 或 `float4`。
* `NORMAL` 是顶点法线，通常为 `float3`。
* `TEXCOORD0` 是第一个 UV 坐标，通常为 `float2`、`float3` 或 `float4`。
* `TEXCOORD1`、`TEXCOORD2` 和 `TEXCOORD3` 分别是第 2、第 3 和第 4 个 UV 坐标。
* `TANGENT` 是切线矢量（用于法线贴图），通常为 `float4`。
* `COLOR` 是每顶点颜色，通常为 `float4`。

当网格数据包含的分量少于顶点着色器输入所需
的分量时，其余部分用零填充，但默认值为 1 的 `.w`
 分量除外。例如，网格纹理坐标
通常是仅包含 x 和 y 分量的 2D 矢量。如果
顶点着色器使用 `TEXCOORD0` 语义声明一个 `float4` 输入，则
顶点着色器接收的值将包含 (x,y,0,1)。

##示例

###可视化 UV

以下着色器示例使用顶点位置和第一个纹理坐标作为顶点着色器输入（在结构 __appdata__ 中定义）。此着色器对于调试网格的 UV 坐标非常有用。



````
Shader "Debug/UV 1" {
SubShader {
    Pass {
		CGPROGRAM
		#pragma vertex vert
		#pragma fragment frag
	#include "UnityCG.cginc"

		// 顶点输入：位置、UV
		struct appdata {
			float4 vertex : POSITION;
			float4 texcoord : TEXCOORD0;
		};

		struct v2f {
			float4 pos : SV_POSITION;
			float4 uv : TEXCOORD0;
		};
		
		v2f vert (appdata v) {
			v2f o;
			o.pos = UnityObjectToClipPos(v.vertex );
			o.uv = float4( v.texcoord.xy, 0, 0 );
			return o;
		}
		
		half4 frag( v2f i ) : SV_Target {
			half4 c = frac( i.uv );
			if (any(saturate(i.uv) - i.uv))
				c.b = 0.5;
			return c;
		}
		ENDCG
    }
}
}
````

在这里，UV 坐标可视化为红色和绿色，而另外的蓝色色调已应用于 0 到 1 范围之外的坐标：

![调试应用于圆环结模型的 UV1 着色器](../uploads/Main/SL-DebugUV1.png)

同样，以下着色器可视化该模型的第二个 UV 集：


````
Shader "Debug/UV 2" {
SubShader {
    Pass {
		CGPROGRAM
		#pragma vertex vert
		#pragma fragment frag
	#include "UnityCG.cginc"

		// 顶点输入：位置、第二个 UV
		struct appdata {
			float4 vertex : POSITION;
			float4 texcoord1 : TEXCOORD1;
		};

		struct v2f {
			float4 pos : SV_POSITION;
			float4 uv : TEXCOORD0;
		};
		
		v2f vert (appdata v) {
			v2f o;
			o.pos = UnityObjectToClipPos(v.vertex );
			o.uv = float4( v.texcoord1.xy, 0, 0 );
			return o;
		}
		
		half4 frag( v2f i ) : SV_Target {
			half4 c = frac( i.uv );
			if (any(saturate(i.uv) - i.uv))
				c.b = 0.5;
			return c;
		}
		ENDCG
    }
}
}
````

###可视化顶点颜色

以下着色器使用顶点位置和每顶点颜色作为顶点着色器输入（在结构 __appdata__ 中定义）。



````
Shader "Debug/Vertex color" {
SubShader {
    Pass {
		CGPROGRAM
		#pragma vertex vert
		#pragma fragment frag
	#include "UnityCG.cginc"

		// 顶点输入：位置、颜色
		struct appdata {
			float4 vertex : POSITION;
			fixed4 color : COLOR;
		};

		struct v2f {
			float4 pos : SV_POSITION;
			fixed4 color : COLOR;
		};
		
		v2f vert (appdata v) {
			v2f o;
			o.pos = UnityObjectToClipPos(v.vertex );
			o.color = v.color;
			return o;
		}
		
		fixed4 frag (v2f i) : SV_Target { return i.color; }
		ENDCG
    }
}
}
````


![调试应用于环形结模型的颜色着色器，其中已将光照烘焙成颜色](../uploads/Main/SL-DebugColors.png)


###可视化法线

以下着色器使用顶点位置和法线作为顶点着色器输入（在结构 __appdata__ 中定义）。法线的 X、Y 和 Z 分量可视化为 RGB 颜色。由于法线分量在 -1 到 1 范围内，我们对这些分量进行缩放和偏置，使输出颜色可在 0 到 1 范围内显示。

````
Shader "Debug/Normals" {
SubShader {
    Pass {
		CGPROGRAM
		#pragma vertex vert
		#pragma fragment frag
	#include "UnityCG.cginc"

		// 顶点输入：位置、法线
		struct appdata {
			float4 vertex : POSITION;
			float3 normal : NORMAL;
		};

		struct v2f {
			float4 pos : SV_POSITION;
			fixed4 color : COLOR;
		};
		
		v2f vert (appdata v) {
			v2f o;
			o.pos = UnityObjectToClipPos(v.vertex );
			o.color.xyz = v.normal * 0.5 + 0.5;
			o.color.w = 1.0;
			return o;
		}
		
		fixed4 frag (v2f i) : SV_Target { return i.color; }
		ENDCG
    }
}
}
````


![调试应用于圆环结模型的法线着色器。您可以看到模型具有硬着色边缘。](../uploads/Main/SL-DebugNormals.png)


###可视化切线和副法线

切线和副法线矢量用于法线贴图。在 Unity 中，只有切线矢量存储在顶点中，而副法线是从法线值和切线值中推导出的。

以下着色器使用顶点位置和切线作为顶点着色器输入（在结构 __appdata__ 中定义）。切线的 x、y 和 z 分量可视化为 RGB 颜色。由于法线分量在 -1 到 1 范围内，我们对这些分量进行缩放和偏置，使输出颜色可在 0 到 1 范围内显示。



````
Shader "Debug/Tangents" {
SubShader {
    Pass {
		CGPROGRAM
		#pragma vertex vert
		#pragma fragment frag
	#include "UnityCG.cginc"

		// 顶点输入：位置、切线
		struct appdata {
			float4 vertex : POSITION;
			float4 tangent : TANGENT;
		};

		struct v2f {
			float4 pos : SV_POSITION;
			fixed4 color : COLOR;
		};
		
		v2f vert (appdata v) {
			v2f o;
			o.pos = UnityObjectToClipPos(v.vertex );
			o.color = v.tangent * 0.5 + 0.5;
			return o;
		}
		
		fixed4 frag (v2f i) : SV_Target { return i.color; }
		ENDCG
    }
}
}
````


![调试应用于圆环结模型的切线着色器。](../uploads/Main/SL-DebugTangents.png)

以下着色器可视化副切线。它使用顶点位置、法线和切线值作为顶点输入。副切线（有时称为
副法线）是根据法线和切线值计算出的。它需要缩放并偏置到可显示的 0 到 1 范围内。



````
Shader "Debug/Bitangents" {
SubShader {
    Pass {
        Fog { Mode Off }
		CGPROGRAM
		#pragma vertex vert
		#pragma fragment frag
	#include "UnityCG.cginc"

		// 顶点输入：位置、法线、切线
		struct appdata {
			float4 vertex : POSITION;
			float3 normal : NORMAL;
			float4 tangent : TANGENT;
		};

		struct v2f {
			float4 pos : SV_POSITION;
			float4 color : COLOR;
		};
		
		v2f vert (appdata v) {
			v2f o;
			o.pos = UnityObjectToClipPos(v.vertex );
			// 计算双切线
			float3 bitangent = cross( v.normal, v.tangent.xyz ) * v.tangent.w;
			o.color.xyz = bitangent * 0.5 + 0.5;
			o.color.w = 1.0;
			return o;
		}
		
		fixed4 frag (v2f i) : SV_Target { return i.color; }
		ENDCG
    }
}
}
````


![调试应用于圆环结模型的副切线着色器。](../uploads/Main/SL-DebugBinormals.png)


## 阅读更多信息

* [着色器语义](SL-ShaderSemantics.html)
* [顶点和片元程序示例](SL-VertexFragmentShaderExamples.html)
* [内置着色器 Include 文件](SL-BuiltinIncludes.html)
