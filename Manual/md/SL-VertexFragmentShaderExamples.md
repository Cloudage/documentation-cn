# 顶点和片元着色器示例

本页面包含顶点和片元程序示例。
有关着色器的基本介绍，请参阅着色器教程：
[第 1 部分](ShaderTut1.html)和[第 2 部分](ShaderTut2.html)。有关编写常规材质着色器的简便方法，请参阅[表面着色器](SL-SurfaceShaders.html)。

可通过 [Unity 项目压缩包](../uploads/Examples/UnityShaderDocExamples.zip)的形式下载以下显示的示例。


### 设置场景

如果您不熟悉 Unity 的 __Scene 视图__、__Hierarchy 视图__、
__Project 视图__和__检视面板 (Inspector)__，现在最好是阅读
手册的前几部分，从 [Unity 基础知识](UnityBasics.html)开始。

第一步是创建一些用于测试着色器的对象。在主菜单中选择 __Game Object__ &gt; __3D Object__ &gt; __Capsule__。然后，调整摄像机位置，使其显示胶囊体。在 Hierarchy 视图中双击胶囊体 (Capsule)
以将 Scene 视图聚焦在该胶囊体上，然后选择主摄像机 (Main Camera) 对象，并单击主菜单中的
 __Game object__ &gt; __Align with View__。

![](../uploads/Main/SL-CapsuleSetup.png) 

在 Project 视图中，从菜单中选择 __Create__ &gt; __Material__，创建新的[材质](Materials.html)。Project 视图中将显示一个名为 _New Material_ 的新材质。

![](../uploads/Main/SL-NewMaterial.png) 

### 创建着色器

现在以类似方式创建一个新的[着色器](Shaders.html)资源。在 Project 视图中，从菜单中选择 __Create__ &gt; __Shader__ &gt; __Unlit Shader__。
随后将创建一个基本着色器，该着色器仅显示一个纹理，无任何光照。

![](../uploads/Main/SL-NewShader.png) 

__Create__ > __Shader__ 菜单中的其他条目将创建基本要素着色器
或其他类型，例如，基本[表面着色器](SL-SurfaceShaders.html)。

### 链接网格、材质和着色器

通过材质的检视面板使材质使用着色器，或者在 Project 视图中将着色器资源拖动到材质资源上。材质检视面板在使用此着色器时将显示白色球体。

![](../uploads/Main/SL-SelectShaderMenu.png) 

现在将材质拖动到 Scene 或 Hierarchy 视图中的网格对象上。或者，选择对象，并在检视面板中使其使用[网格渲染器 (Mesh Renderer)](class-MeshRenderer.html) 组件的 Materials 字段中的材质。

![](../uploads/Main/SL-WhiteCapsuleUnlitShader.png) 

完整这些设置后，现在可以开始查看着色器代码，并会在 Scene 视图中看到对胶囊体上着色器所做的更改的结果。

## 着色器的主要部分

要开始检查着色器的代码，请在 Project 视图中双击着色器资源。着色器代码将在脚本编辑器（MonoDevelop 或 Visual Studio）中打开。

着色器最开始的代码如下：

```
Shader "Unlit/NewUnlitShader"
{
	Properties
	{
		_MainTex ("Texture", 2D) = "white" {}
	}
	SubShader
	{
		Tags { "RenderType"="Opaque" }
		LOD 100

		Pass
		{
			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			// 使雾生效
			#pragma multi_compile_fog
			
			#include "UnityCG.cginc"

			struct appdata
			{
				float4 vertex : POSITION;
				float2 uv : TEXCOORD0;
			};

			struct v2f
			{
				float2 uv : TEXCOORD0;
				UNITY_FOG_COORDS(1)
				float4 vertex : SV_POSITION;
			};

			sampler2D _MainTex;
			float4 _MainTex_ST;
			
			v2f vert (appdata v)
			{
				v2f o;
				o.vertex = UnityObjectToClipPos(v.vertex);
				o.uv = TRANSFORM_TEX(v.uv, _MainTex);
				UNITY_TRANSFER_FOG(o,o.vertex);
				return o;
			}
			
			fixed4 frag (v2f i) : SV_Target
			{
				// 对纹理进行采样
				fixed4 col = tex2D(_MainTex, i.uv);
				// 应用雾
				UNITY_APPLY_FOG(i.fogCoord, col);
				return col;
			}
			ENDCG
		}
	}
}
```

这个初始着色器看上去不是那么简单！但不要担心，
我们将逐步介绍每个部分。

让我们看看这个简单着色器的主要部分。

    Shader

[Shader](SL-Shader.html) 命令包含一个带有
着色器名称的字符串。在[材质](class-Material.html)检视面板中选择着色器时，可以使用正斜杠字符“/”将着色器放置在子菜单中。

    Properties

[Properties](SL-Properties.html) 代码块包含着色器变量
（纹理、颜色等），这些变量将保存为材质的一部分，
并显示在材质检视面板中。在我们的无光照着色器模板中，
声明了单个纹理属性。

    SubShader

一个着色器 (Shader) 可以包含一个或多个[子着色器 (SubShader)](SL-SubShader.html)，主要
用于实现不同 GPU 功能的着色器。
在本教程中，我们并不太关心这一点，所以我们
所有的着色器都只包含一个子着色器。

    Pass

每个子着色器由多个[通道](SL-Pass.html)组成，每个通道代表
为使用着色器材质渲染的同一对象执行
顶点和片元代码。
许多简单的着色器只使用一个通道，但与光照交互的
着色器可能需要更多通道（有关详细信息，请参阅
[光照管线](SL-RenderPipeline.html)）。通道内部的
命令通常设置固定函数状态，例如
混合模式。

    CGPROGRAM ..ENDCG

这些关键字包裹着顶点和片元着色器中的 HLSL 代码
部分。通常，大多数相关代码都包含在这里。有关详细信息，请参阅
[顶点和片元着色器](SL-ShaderPrograms.html)。


## 简单的无光照着色器

无光照着色器模板比显示带纹理的对象
绝对需要更多的代码。例如，
它支持雾效，以及材质中的纹理平铺/偏移字段。
让我们最大程度简化着色器，并添加更多注释：


```
Shader "Unlit/SimpleUnlitTexturedShader"
{
    Properties
    {
        // 我们已删除对纹理平铺/偏移的支持，
        // 因此请让它们不要显示在材质检视面板中
        [NoScaleOffset] _MainTex ("Texture", 2D) = "white" {}
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            // 使用 "vert" 函数作为顶点着色器
            #pragma vertex vert
            // 使用 "frag" 函数作为像素（片元）着色器
            #pragma fragment frag

            // 顶点着色器输入
            struct appdata
            {
                float4 vertex : POSITION; // 顶点位置
                float2 uv : TEXCOORD0; // 纹理坐标
            };

            // 顶点着色器输出（"顶点到片元"）
            struct v2f
            {
                float2 uv : TEXCOORD0; // 纹理坐标
                float4 vertex : SV_POSITION; // 裁剪空间位置
            };

            // 顶点着色器
            v2f vert (appdata v)
            {
                v2f o;
                // 将位置转换为裁剪空间
                //（乘以模型*视图*投影矩阵）
                o.vertex = mul(UNITY_MATRIX_MVP, v.vertex);
                // 仅传递纹理坐标
                o.uv = v.uv;
                return o;
            }
            
            // 我们将进行采样的纹理
            sampler2D _MainTex;

            // 像素着色器；返回低精度（"fixed4" 类型）
            // 颜色（"SV_Target" 语义）
            fixed4 frag (v2f i) : SV_Target
            {
                // 对纹理进行采样并将其返回
                fixed4 col = tex2D(_MainTex, i.uv);
                return col;
            }
            ENDCG
        }
    }
}
```

__顶点着色器__是对 3D 模型的每个顶点运行的程序。通常情况下，它并不做任何特别有趣的事情。这里我们只是将顶点位置从对象空间转换为所谓的“裁剪空间”（由 GPU 用于栅格化屏幕上的对象）。我们还传递未修改的输入纹理坐标；我们需要该坐标来对片元着色器中的纹理进行采样。

__片元着色器__是对屏幕上对象占据的每个像素运行的程序，通常用于计算和输出每个像素的颜色。通常，屏幕上有数百万个像素，并且片元着色器将
针对所有像素执行！优化片元着色器是整体游戏性能优化工作的重要组成部分。

一些变量或函数定义后跟一个**语义指示符**，例如 __: POSITION__ 或 __: SV_Target__。这些语义指示符将这些变量的“含义”传达给 GPU。有关详细信息，请参阅[着色器语义](SL-ShaderSemantics.html)页面。

用于具有良好纹理的良好模型时，我们的简单着色器看起来非常好！

![](../uploads/SL/ExampleUnlitTextured.png) 


## 更简单的单色着色器

让我们进一步简化着色器：我们将制作一个用单一颜色绘制整个对象的
着色器。这并不是很有用，但别忘了，我们是在学习。

```
Shader "Unlit/SingleColor"
{
    Properties
    {
        // 材质检视面板的颜色属性，默认为白色
        _Color ("Main Color", Color) = (1,1,1,1)
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            
            // 顶点着色器
            // 这次不使用 "appdata" 结构，仅手动拼写输入，
            // 并且不返回 v2f 结构，同样仅返回单个输出
            // float4 裁剪位置
            float4 vert (float4 vertex : POSITION) : SV_POSITION
            {
                return mul(UNITY_MATRIX_MVP, vertex);
            }
            
            // 来自材质的颜色
            fixed4 _Color;

            // 像素着色器，无需输入
            fixed4 frag () : SV_Target
            {
                return _Color; // 仅将其返回
            }
            ENDCG
        }
    }
}
```

这次，着色器函数只是手动拼出输入，而不是使用输入结构 (__appdata__) 和输出结构 (__v2f__)。两种方式都有效，选择使用哪种方式取决于您编写代码的风格和偏好。

![](../uploads/SL/ExampleSingleColor.png) 


## 使用网格法线，轻松获利

让我们继续了解在世界空间中显示网格法线的着色器。言归正传：

```
Shader "Unlit/WorldSpaceNormals"
{
    // 这次没有 Properties 代码块！
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            // 包含 UnityObjectToWorldNormal helper 函数的 include 文件
            #include "UnityCG.cginc"

            struct v2f {
                // 我们将输出世界空间法线作为常规 ("texcoord") 插值器之一
                half3 worldNormal : TEXCOORD0;
                float4 pos : SV_POSITION;
            };

            // 顶点着色器：将对象空间法线也作为输入
            v2f vert (float4 vertex : POSITION, float3 normal : NORMAL)
            {
                v2f o;
                o.pos = UnityObjectToClipPos(vertex);
                // UnityCG.cginc 文件包含将法线从对象变换到
                // 世界空间的函数，请使用该函数
                o.worldNormal = UnityObjectToWorldNormal(normal);
                return o;
            }
            
            fixed4 frag (v2f i) : SV_Target
            {
                fixed4 c = 0;
                // 法线是具有 xyz 分量的 3D 矢量；处于 -1..1
                // 范围。要将其显示为颜色，请将此范围设置为 0..1
                // 并放入红色、绿色、蓝色分量
                c.rgb = i.worldNormal*0.5+0.5;
                return c;
            }
            ENDCG
        }
    }
}
```

![](../uploads/SL/ExampleWorldSpaceNormals.png) 

除了产生漂亮的颜色外，法线还用于各种图形效果：光照、反射、轮廓等。

在上面的着色器中，我们开始使用 Unity 的内置[着色器 include 文件](SL-BuiltinIncludes.html)之一。
这里使用的 __UnityCG.cginc__ 包含一个方便的函数 __UnityObjectToWorldNormal__。我们还使用了实用函数 __UnityObjectToClipPos__，该函数将顶点从对象空间转换为屏幕。这使得代码更易于阅读，并且在某些情况下更有效。

在所谓的“插值器”（有时称为“变化”）中，我们已经看到数据可从顶点传入片元着色器。在 HLSL 着色语言中，它们通常用 __TEXCOORDn__ 语义进行标记，其中每一个最多可以是一个 4 分量矢量（有关详细信息，请参阅[语义](SL-ShaderSemantics.html)页面）。

此外，我们学习了如何使用一种简单的技术将标准化矢量（在 -1.0 到 +1.0 范围内）可视化为颜色：只需将它们乘以二分之一并加二分之一。请在[顶点程序输入](SL-VertexProgramInputs.html)页面中查看更多顶点数据可视化示例。


#### 使用世界空间法线的环境反射

如果在场景中使用[天空盒](class-Skybox.html)作为反射源（请参阅 [Lighting 窗口](GlobalIllumination.html)），
基本上会创建一个包含天空盒数据的“默认”[反射探针](class-ReflectionProbe.html)。
反射探针内部是[立方体贴图](class-Cubemap.html)纹理；我们将扩展上面的世界空间法线着色器来对其进行深入了解。

代码现在开始变得有点复杂。当然，如果您希望着色器能够自动处理光源、阴影、反射以及光照系统的其余部分，使用[表面着色器](SL-SurfaceShaders.html)会容易得多。此示例旨在向您展示如何以“手动”方式使用光照系统的各个部分。

```
Shader "Unlit/SkyReflection"
{
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            struct v2f {
                half3 worldRefl : TEXCOORD0;
                float4 pos : SV_POSITION;
            };

            v2f vert (float4 vertex : POSITION, float3 normal : NORMAL)
            {
                v2f o;
                o.pos = UnityObjectToClipPos(vertex);
                // 计算顶点的世界空间位置
                float3 worldPos = mul(_Object2World, vertex).xyz;
                // 计算世界空间视图方向
                float3 worldViewDir = normalize(UnityWorldSpaceViewDir(worldPos));
                // 世界空间法线
                float3 worldNormal = UnityObjectToWorldNormal(normal);
                // 世界空间反射矢量
                o.worldRefl = reflect(-worldViewDir, worldNormal);
                return o;
            }
        
            fixed4 frag (v2f i) : SV_Target
            {
                // 使用反射矢量对默认反射立方体贴图进行采样
                half4 skyData = UNITY_SAMPLE_TEXCUBE(unity_SpecCube0, i.worldRefl);
                // 将立方体贴图数据解码为实际颜色
                half3 skyColor = DecodeHDR (skyData, unity_SpecCube0_HDR);
                // 将其输出！
                fixed4 c = 0;
                c.rgb = skyColor;
                return c;
            }
            ENDCG
        }
    }
}
```

![](../uploads/SL/ExampleSkyReflection.png) 

以上示例使用了内置[着色器 include 文件](SL-BuiltinIncludes.html)中的部分内容：

* 来自[内置着色器变量](SL-UnityShaderVariables.html)的 __unity_SpecCube0__、__unity_SpecCube0_HDR__、__Object2World__ 和 
__UNITY_MATRIX_MVP__。__unity_SpecCube0__ 包含激活的反射探针的数据。
* __UNITY_SAMPLE_TEXCUBE__ 是一个用于采样立方体贴图的[内置宏](SL-BuiltinMacros.html)。通常使用标准 HLSL 语法声明和
使用大多数常规立方体贴图（__samplerCUBE__ 和 __texCUBE__），但 Unity 中的反射探针立方体贴图以特殊方式声明以节省采样器字段。如果您不知道这是什么，请不要担心，只需要知道：要使用 __unity_SpecCube0__ 立方体贴图，必须使用 __UNITY_SAMPLE_TEXCUBE__ 宏。
* __UnityCG.cginc__ 中的 __UnityWorldSpaceViewDir__ 函数以及来自同一文件的 __DecodeHDR__ 函数。后者用于从反射探针数据中获取实际颜色（因为 Unity 以特殊编码方式存储反射探针立方体贴图）。
* __reflect__ 只是一个内置的 HLSL 函数，用于计算给定法线周围的矢量反射。


#### 使用法线贴图的环境反射

通常，__法线贴图__用于在对象上创建其他细节，而无需创建其他几何体。让我们看看如何使用法线贴图纹理制作发射环境的着色器。

现在开始*真正涉及到了*数学知识，所以我们将分几步完成。在上面的着色器中，反射
方向是按每个顶点计算的（在顶点着色器中），片元着色器仅执行反射探针
立方体贴图查找。然而，一旦我们开始使用法线贴图，表面法线本身需要按每个像素计算，这意味着我们还必须按每个像素计算环境的反射方式！

首先，让我们重写上面的着色器来实现相同效果，只是我们将一些计算移到片元着色器，从而按每个像素进行计算：

```
Shader "Unlit/SkyReflection Per Pixel"
{
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            struct v2f {
                float3 worldPos : TEXCOORD0;
                half3 worldNormal : TEXCOORD1;
                float4 pos : SV_POSITION;
            };

            v2f vert (float4 vertex : POSITION, float3 normal : NORMAL)
            {
                v2f o;
                o.pos = UnityObjectToClipPos(vertex);
                o.worldPos = mul(_Object2World, vertex).xyz;
                o.worldNormal = UnityObjectToWorldNormal(normal);
                return o;
            }
        
            fixed4 frag (v2f i) : SV_Target
            {
                // 计算视图方向和反射矢量
                // 此处为每像素计算
                half3 worldViewDir = normalize(UnityWorldSpaceViewDir(i.worldPos));
                half3 worldRefl = reflect(-worldViewDir, i.worldNormal);

                // 与在先前的着色器中相同
                half4 skyData = UNITY_SAMPLE_TEXCUBE(unity_SpecCube0, worldRefl);
                half3 skyColor = DecodeHDR (skyData, unity_SpecCube0_HDR);
                fixed4 c = 0;
                c.rgb = skyColor;
                return c;
            }
            ENDCG
        }
    }
}
```

这里并没有带给我们太多不同：着色器看起来完全相同，只是它现在运行得更慢，因为它对屏幕上的每个像素进行更多的计算，而不是仅对每个模型的顶点进行计算。但是，我们很快就会真正需要这些计算。更高的图形保真度通常需要更复杂的着色器。

我们现在也要学习新东西，即所谓的“切线空间”。法线贴图纹理通常在坐标空间中表示，可将其视为“跟随模型表面”。在我们的着色器中，我们需要知道切线空间基矢量，从纹理中读取法线矢量，将其转换为世界空间，然后通过上面的着色器
进行所有数学运算。我们行动吧！

```
Shader "Unlit/SkyReflection Per Pixel"
{
    Properties {
        // 材质上的法线贴图纹理，
        // 默认为虚拟的 "平面表面" 法线贴图
        _BumpMap("Normal Map", 2D) = "bump" {}
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            struct v2f {
                float3 worldPos : TEXCOORD0;
                // 这三个矢量将保持一个 3x3 旋转矩阵，
                // 此矩阵进行从切线到世界空间的转换
                half3 tspace0 : TEXCOORD1; // tangent.x, bitangent.x, normal.x
                half3 tspace1 : TEXCOORD2; // tangent.y, bitangent.y, normal.y
                half3 tspace2 : TEXCOORD3; // tangent.z, bitangent.z, normal.z
                // 法线贴图的纹理坐标
                float2 uv : TEXCOORD4;
                float4 pos : SV_POSITION;
            };

            // 顶点着色器现在还需要每顶点切线矢量。
            // 在 Unity 中，切线为 4D 矢量，其中使用 .w 分量
            // 指示双切线矢量的方向。
            // 我们还需要纹理坐标。
            v2f vert (float4 vertex : POSITION, float3 normal : NORMAL, float4 tangent : TANGENT, float2 uv : TEXCOORD0)
            {
                v2f o;
                o.pos = UnityObjectToClipPos(vertex);
                o.worldPos = mul(_Object2World, vertex).xyz;
                half3 wNormal = UnityObjectToWorldNormal(normal);
                half3 wTangent = UnityObjectToWorldDir(tangent.xyz);
                // 从法线和切线的交叉积计算双切线
                half tangentSign = tangent.w * unity_WorldTransformParams.w;
                half3 wBitangent = cross(wNormal, wTangent) * tangentSign;
                // 输出切线空间矩阵
                o.tspace0 = half3(wTangent.x, wBitangent.x, wNormal.x);
                o.tspace1 = half3(wTangent.y, wBitangent.y, wNormal.y);
                o.tspace2 = half3(wTangent.z, wBitangent.z, wNormal.z);
                o.uv = uv;
                return o;
            }

            // 来自着色器属性的法线贴图纹理
            sampler2D _BumpMap;
        
            fixed4 frag (v2f i) : SV_Target
            {
                // 对法线贴图进行采样，并根据 Unity 编码进行解码
                half3 tnormal = UnpackNormal(tex2D(_BumpMap, i.uv));
                // 将法线从切线变换到世界空间
                half3 worldNormal;
                worldNormal.x = dot(i.tspace0, tnormal);
                worldNormal.y = dot(i.tspace1, tnormal);
                worldNormal.z = dot(i.tspace2, tnormal);

                // 与在先前的着色器中处于相同的位置
                half3 worldViewDir = normalize(UnityWorldSpaceViewDir(i.worldPos));
                half3 worldRefl = reflect(-worldViewDir, worldNormal);
                half4 skyData = UNITY_SAMPLE_TEXCUBE(unity_SpecCube0, worldRefl);
                half3 skyColor = DecodeHDR (skyData, unity_SpecCube0_HDR);
                fixed4 c = 0;
                c.rgb = skyColor;
                return c;
            }
            ENDCG
        }
    }
}
```

哎呦，这个非常复杂。但是，看看法线贴图反射！


![](../uploads/SL/ExampleSkyReflectionNormalmap.png) 


## 添加更多纹理

让我们为上面的法线贴图、天空反射着色器添加更多纹理。我们将添加基础颜色纹理（如第一个无光照的示例中所示）和遮挡贴图来使洞穴变暗。

```
Shader "Unlit/More Textures"
{
    Properties {
        // 我们将在材质中使用三种纹理
        _MainTex("Base texture", 2D) = "white" {}
        _OcclusionMap("Occlusion", 2D) = "white" {}
        _BumpMap("Normal Map", 2D) = "bump" {}
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            // 与在先前的着色器中完全相同
            struct v2f {
                float3 worldPos : TEXCOORD0;
                half3 tspace0 : TEXCOORD1;
                half3 tspace1 : TEXCOORD2;
                half3 tspace2 : TEXCOORD3;
                float2 uv : TEXCOORD4;
                float4 pos : SV_POSITION;
            };
            v2f vert (float4 vertex : POSITION, float3 normal : NORMAL, float4 tangent : TANGENT, float2 uv : TEXCOORD0)
            {
                v2f o;
                o.pos = UnityObjectToClipPos(vertex);
                o.worldPos = mul(_Object2World, vertex).xyz;
                half3 wNormal = UnityObjectToWorldNormal(normal);
                half3 wTangent = UnityObjectToWorldDir(tangent.xyz);
                half tangentSign = tangent.w * unity_WorldTransformParams.w;
                half3 wBitangent = cross(wNormal, wTangent) * tangentSign;
                o.tspace0 = half3(wTangent.x, wBitangent.x, wNormal.x);
                o.tspace1 = half3(wTangent.y, wBitangent.y, wNormal.y);
                o.tspace2 = half3(wTangent.z, wBitangent.z, wNormal.z);
                o.uv = uv;
                return o;
            }

            // 来自着色器属性的纹理
            sampler2D _MainTex;
            sampler2D _OcclusionMap;
            sampler2D _BumpMap;
        
            fixed4 frag (v2f i) : SV_Target
            {
                // 与在先前的着色器中相同...
                half3 tnormal = UnpackNormal(tex2D(_BumpMap, i.uv));
                half3 worldNormal;
                worldNormal.x = dot(i.tspace0, tnormal);
                worldNormal.y = dot(i.tspace1, tnormal);
                worldNormal.z = dot(i.tspace2, tnormal);
                half3 worldViewDir = normalize(UnityWorldSpaceViewDir(i.worldPos));
                half3 worldRefl = reflect(-worldViewDir, worldNormal);
                half4 skyData = UNITY_SAMPLE_TEXCUBE(unity_SpecCube0, worldRefl);
                half3 skyColor = DecodeHDR (skyData, unity_SpecCube0_HDR);                
                fixed4 c = 0;
                c.rgb = skyColor;

                // 使用基本纹理和遮挡贴图调制天空颜色
                fixed3 baseColor = tex2D(_MainTex, i.uv).rgb;
                fixed occlusion = tex2D(_OcclusionMap, i.uv).r;
                c.rgb *= baseColor;
                c.rgb *= occlusion;

                return c;
            }
            ENDCG
        }
    }
}
```

气球猫看起来很不错！

![](../uploads/SL/ExampleMoreTextures.png) 



## 纹理化着色器示例

#### 程序化棋盘图案

下面的着色器根据网格的纹理坐标输出棋盘图案：

```
Shader "Unlit/Checkerboard"
{
    Properties
    {
        _Density ("Density", Range(2,50)) = 30
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            struct v2f
            {
                float2 uv : TEXCOORD0;
                float4 vertex : SV_POSITION;
            };

            float _Density;

            v2f vert (float4 pos : POSITION, float2 uv : TEXCOORD0)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(pos);
                o.uv = uv * _Density;
                return o;
            }
            
            fixed4 frag (v2f i) : SV_Target
            {
                float2 c = i.uv;
                c = floor(c) / 2;
                float checker = frac(c.x + c.y) * 2;
                return checker;
            }
            ENDCG
        }
    }
}
```

[Properties](SL-Properties.html) 代码块中的密度滑动条控制棋盘的密集程度。在顶点着色器中，网格 UV 与密度值相乘，使它们从 0 到 1 的范围变为 0 到密度的范围。假设密度设置为 30，这将使片元着色器中的 __i.uv__ 输入包含 0 到 30 的浮点值，对应于正在渲染的网格的各个位置。

然后，片元着色器代码仅使用 HLSL 的内置 __floor__ 函数获取输入坐标的整数部分，并将其除以 2。回想一下，输入坐标是从 0 到 30 的数字；这使得它们都被“量化”为 0、0.5、1、1.5、2、2.5 等等的值。输入坐标的 x 和 y 分量都完成了此操作。

接下来，我们将这些 x 和 y 坐标相加（每个坐标的可能值只有 0、0.5、1、1.5 等等），并且只使用另一个内置的 HLSL 函数 __frac__ 来获取小数部分。结果只能是 0.0 或 0.5。然后，我们将它乘以 2 使其为 0.0 或 1.0，并输出为颜色（这分别产生黑色或白色）。

![](../uploads/SL/ExampleCheckerboard.png) 


#### 三面纹理

对于复杂网格或程序化网格，不使用常规 UV 坐标对它们进行纹理化，有时只需在三个主方向将纹理“投影”到对象上即可。这称为“三面”纹理。这个想法是使用表面法线来加权三个纹理方向。下面是着色器：

```
Shader "Unlit/Triplanar"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        _Tiling ("Tiling", Float) = 1.0
        _OcclusionMap("Occlusion", 2D) = "white" {}
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            struct v2f
            {
                half3 objNormal : TEXCOORD0;
                float3 coords : TEXCOORD1;
                float2 uv : TEXCOORD2;
                float4 pos : SV_POSITION;
            };

            float _Tiling;

            v2f vert (float4 pos : POSITION, float3 normal : NORMAL, float2 uv : TEXCOORD0)
            {
                v2f o;
                o.pos = UnityObjectToClipPos(pos);
                o.coords = pos.xyz * _Tiling;
                o.objNormal = normal;
                o.uv = uv;
                return o;
            }

            sampler2D _MainTex;
            sampler2D _OcclusionMap;
            
            fixed4 frag (v2f i) : SV_Target
            {
                // 使用法线的绝对值作为纹理权重
                half3 blend = abs(i.objNormal);
                // 确保权重之和为 1（除以 x+y+z 之和）
                blend /= dot(blend,1.0);
                // 针对 x、y、z 轴读取三个纹理投影
                fixed4 cx = tex2D(_MainTex, i.coords.yz);
                fixed4 cy = tex2D(_MainTex, i.coords.xz);
                fixed4 cz = tex2D(_MainTex, i.coords.xy);
                // 根据权重混合纹理
                fixed4 c = cx * blend.x + cy * blend.y + cz * blend.z;
                // 根据常规遮挡贴图进行调制
                c *= tex2D(_OcclusionMap, i.uv);
                return c;
            }
            ENDCG
        }
    }
}
```

![](../uploads/SL/ExampleTriPlanar.png) 


## 光照计算

通常，当您需要适用于 Unity 光照管线的着色器时，
可以编写[表面着色器](SL-SurfaceShaders.html)。这样会为您完成大部分“繁重的工作”，
您的着色器代码只需要定义表面属性。

但在某些情况下，您希望绕过标准表面着色器路径；
或者是出于性能原因，您只想支持整个光照管线的某个有限子集，
或者您想要进行“标准光照”以外的自定义。以下示例
将说明如何从手动编写的顶点和片元着色器获取光照数据。
查看表面着色器生成的代码（通过[着色器检视面板](class-Shader.html)）也是一种
很好的学习资源。


#### 简单的漫射光照

我们需要做的第一件事就是指出我们的着色器确实需要传递给它的光照信息。Unity 的[渲染管线](SL-RenderPipeline.html)支持各种渲染方式；这里我们将使用默认的[前向渲染](RenderTech-ForwardRendering.html)。

我们首先只支持一个方向光。Unity 中的前向渲染的工作方式是在一个名为 __ForwardBase__ 的单个通道中渲染主方向光、环境光、光照贴图和反射。在着色器中，通过添加以下[通道标签](SL-PassTags.html)来指示这一情况：__Tags {"LightMode"="ForwardBase"}__。这将使方向光数据通过一些[内置变量](SL-UnityShaderVariables.html)传入着色器。

以下着色器为每个顶点计算简单漫射光照，并使用单个主纹理：

```
Shader "Lit/Simple Diffuse"
{
    Properties
    {
        [NoScaleOffset] _MainTex ("Texture", 2D) = "white" {}
    }
    SubShader
    {
        Pass
        {
            // 指示我们的通道是前向渲染管线中的
            // "基础" 通道。它可设置环境光和主
            // 方向光数据；光线方向为 _WorldSpaceLightPos0
            // 而颜色为 _LightColor0
	Tags {"LightMode"="ForwardBase"}
        
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc" // 对于 UnityObjectToWorldNormal
            #include "UnityLightingCommon.cginc" // 对于 _LightColor0

            struct v2f
            {
                float2 uv : TEXCOORD0;
                fixed4 diff : COLOR0; // 漫射光照颜色
                float4 vertex : SV_POSITION;
            };

            v2f vert (appdata_base v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = v.texcoord;
                // 在世界空间中获取顶点法线
                half3 worldNormal = UnityObjectToWorldNormal(v.normal);
                // 标准漫射（兰伯特）光照的法线和
                // 光线方向之间的点积
                half nl = max(0, dot(worldNormal, _WorldSpaceLightPos0.xyz));
                // 考虑浅色因素
                o.diff = nl * _LightColor0;
                return o;
            }
            
            sampler2D _MainTex;

            fixed4 frag (v2f i) : SV_Target
            {
                // 样本纹理
                fixed4 col = tex2D(_MainTex, i.uv);
                // 乘以光照
                col *= i.diff;
                return col;
            }
            ENDCG
        }
    }
}
```

这使得对象对光线方向作出反应：面向光源的部分获得光照，而背向的部分完全没有光照。

![](../uploads/SL/ExampleDiffuseLighting.png) 


#### 带环境光的漫射光照

上面的示例不考虑任何环境光照或光照探针。我们来解决这个问题！
事实证明，我们可以通过添加一行代码来实现这一目标。环境光和[光照探针](LightProbes.html)数据都以球谐函数形式传递给着色器，__UnityCG.cginc__ [include 文件](SL-BuiltinIncludes.html) 中的 __ShadeSH9__ 函数在给定世界空间法线的情况下完成所有估算工作。

```
Shader "Lit/Diffuse With Ambient"
{
    Properties
    {
        [NoScaleOffset] _MainTex ("Texture", 2D) = "white" {}
    }
    SubShader
    {
        Pass
        {
	Tags {"LightMode"="ForwardBase"}
        
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"
            #include "UnityLightingCommon.cginc"

            struct v2f
            {
                float2 uv : TEXCOORD0;
                fixed4 diff : COLOR0;
                float4 vertex : SV_POSITION;
            };

            v2f vert (appdata_base v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = v.texcoord;
                half3 worldNormal = UnityObjectToWorldNormal(v.normal);
                half nl = max(0, dot(worldNormal, _WorldSpaceLightPos0.xyz));
                o.diff = nl * _LightColor0;

                // 与先前着色器的唯一区别：
                // 除了来自主光源的漫射光照，
                // 还可添加来自环境或光照探针的光照
                // 来自 UnityCG.cginc 的 ShadeSH9 函数使用世界空间法线
                // 对其进行估算
                o.diff.rgb += ShadeSH9(half4(worldNormal,1));
                return o;
            }
            
            sampler2D _MainTex;

            fixed4 frag (v2f i) : SV_Target
            {
                fixed4 col = tex2D(_MainTex, i.uv);
                col *= i.diff;
                return col;
            }
            ENDCG
        }
    }
}
```

事实上，这个着色器开始看起来非常类似于内置的[旧版漫射](shader-NormalDiffuse.html)着色器！

![](../uploads/SL/ExampleDiffuseAmbientLighting.png) 


#### 实现阴影投射

我们的着色器目前既不能接受也不能投射阴影。让我们先实现阴影投射。

为了投射阴影，着色器必须在其任何[子着色器](SL-SubShader.html)或任何[回退](SL-Fallback.html)中具有 __ShadowCaster__ [通道类型](SL-PassTags.html)。ShadowCaster 通道用于将对象渲染到阴影贴图中，通常它非常简单：顶点着色器只需要估算顶点位置，片元着色器几乎不执行任何操作。阴影贴图只是深度缓冲区，因此即使是片元着色器输出的颜色也无关紧要。

这意味着对于许多着色器，阴影投射物通道几乎完全相同（除非对象具有基于自定义顶点着色器的变形，或者具有 Alpha 镂空/半透明部分）。最简单的捕捉方法是使用着色器命令 [UsePass](SL-UsePass.html)：

```
Pass
{
	// 常规光照通道
}
// 通过 VertexLit 内置着色器捕捉阴影投射物
UsePass "Legacy Shaders/VertexLit/SHADOWCASTER"
```

但我们在这里是为了学习，所以让我们通过“手动”方式实现相同效果。为了缩短代码长度，
我们已经将光照通道 ("ForwardBase") 替换为仅执行无纹理环境光的代码。在它下面，有一个“ShadowCaster”通道，让对象能够支持阴影投射。

```
Shader "Lit/Shadow Casting"
{
    SubShader
    {
        // 非常简单的光照通道，仅处理非纹理环境
        Pass
        {
	Tags {"LightMode"="ForwardBase"}
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"
            struct v2f
            {
                fixed4 diff : COLOR0;
                float4 vertex : SV_POSITION;
            };
            v2f vert (appdata_base v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                half3 worldNormal = UnityObjectToWorldNormal(v.normal);
                // 仅估算环境光
                o.diff.rgb = ShadeSH9(half4(worldNormal,1));
                o.diff.a = 1;
                return o;
            }
            fixed4 frag (v2f i) : SV_Target
            {
                return i.diff;
            }
            ENDCG
        }

        // 阴影投射物渲染通道，
        // 使用 UnityCG.cginc 中的宏手动实现
        Pass
        {
            Tags {"LightMode"="ShadowCaster"}

            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #pragma multi_compile_shadowcaster
            #include "UnityCG.cginc"

            struct v2f { 
                V2F_SHADOW_CASTER;
            };

            v2f vert(appdata_base v)
            {
                v2f o;
                TRANSFER_SHADOW_CASTER_NORMALOFFSET(o)
                return o;
            }

            float4 frag(v2f i) : SV_Target
            {
                SHADOW_CASTER_FRAGMENT(i)
            }
            ENDCG
        }
    }
}
```

现在下方有一个平面，使用常规的内置漫射着色器，因此我们可以看到
我们的阴影生效（请记住，我们当前的着色器还不支持_接受_阴影！）。

![](../uploads/SL/ExampleShadowCasting.png) 

我们使用了 __#pragma multi_compile_shadowcaster__ 指令。这会导致着色器被编译为多个变体，并为每个变体定义了不同的预处理器宏（有关详细信息，请参阅
[多个着色器变体](SL-MultipleProgramVariants.html)页面）。渲染到阴影贴图时，点光源与其他光源类型需要着色器代码略有不同，这就是需要此指令的原因。


#### 接受阴影

要实现对接受阴影的支持，必须将基础光照通道编译成
若干变体，以正确处理“没有阴影的方向光”和“有阴影的方向光”这两种情况。__#pragma multi_compile_fwdbase__ 指令便可执行此操作（有关详细信息，请参阅
[多个着色器变体](SL-MultipleProgramVariants.html)）。实际上它的功能还不止于此：
它还为不同的光照贴图类型编译变体、实时 GI 打开或关闭等。目前我们不需要这些，所以我们将明确跳过这些变体。

然后，为了获得实际的阴影计算，我们将 __#include "AutoLight.cginc"__ 着色器的 [include 文件](SL-BuiltinIncludes.html) 并使用该文件中的 SHADOW_COORDS、TRANSFER_SHADOW 和 SHADOW_ATTENUATION 宏。

下面是着色器：

```
Shader "Lit/Diffuse With Shadows"
{
    Properties
    {
        [NoScaleOffset] _MainTex ("Texture", 2D) = "white" {}
    }
    SubShader
    {
        Pass
        {
	Tags {"LightMode"="ForwardBase"}
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"
            #include "Lighting.cginc"

            // 将着色器编译成多个有阴影和没有阴影的变体
            //（我们还不关心任何光照贴图，所以跳过这些变体）
            #pragma multi_compile_fwdbase nolightmap nodirlightmap nodynlightmap novertexlight
            // 阴影 helper 函数和宏
            #include "AutoLight.cginc"

            struct v2f
            {
                float2 uv : TEXCOORD0;
                SHADOW_COORDS(1) // 将阴影数据放入 TEXCOORD1
                fixed3 diff : COLOR0;
                fixed3 ambient : COLOR1;
                float4 pos : SV_POSITION;
            };
            v2f vert (appdata_base v)
            {
                v2f o;
                o.pos = UnityObjectToClipPos(v.vertex);
                o.uv = v.texcoord;
                half3 worldNormal = UnityObjectToWorldNormal(v.normal);
                half nl = max(0, dot(worldNormal, _WorldSpaceLightPos0.xyz));
                o.diff = nl * _LightColor0.rgb;
                o.ambient = ShadeSH9(half4(worldNormal,1));
                // 计算阴影数据
                TRANSFER_SHADOW(o)
                return o;
            }

            sampler2D _MainTex;

            fixed4 frag (v2f i) : SV_Target
            {
                fixed4 col = tex2D(_MainTex, i.uv);
                // 计算阴影衰减（1.0 = 完全照亮，0.0 = 完全阴影）
                fixed shadow = SHADOW_ATTENUATION(i);
                // 用阴影使光照变暗，保持环境不变
                fixed3 lighting = i.diff * shadow + i.ambient;
                col.rgb *= lighting;
                return col;
            }
            ENDCG
        }

        // 阴影投射支持
        UsePass "Legacy Shaders/VertexLit/SHADOWCASTER"
    }
}
```

看看，现在我们实现了阴影！

![](../uploads/SL/ExampleShadowReceiving.png) 


## 其他着色器示例

###雾效


```
Shader "Custom/TextureCoordinates/Fog" {
    SubShader {
        Pass {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
			
			//编译雾效变化时需要。
			#pragma multi_compile_fog

            #include "UnityCG.cginc"

            struct vertexInput {
                float4 vertex : POSITION;
                float4 texcoord0 : TEXCOORD0;
            };

            struct fragmentInput{
                float4 position : SV_POSITION;
                float4 texcoord0 : TEXCOORD0;
				
				//用于传递雾量，数字应为自由纹理坐标值。
				UNITY_FOG_COORDS(1)
            };

            fragmentInput vert(vertexInput i){
                fragmentInput o;
                o.position = UnityObjectToClipPos(i.vertex);
                o.texcoord0 = i.texcoord0;
				
				//从裁剪空间位置计算雾量。
				UNITY_TRANSFER_FOG(o,o.position);
                return o;
            }

            fixed4 frag(fragmentInput i) : SV_Target {
                fixed4 color = fixed4(i.texcoord0.xy,0,0);
				
				//应用雾效（自动处理附加通道）
                UNITY_APPLY_FOG(i.fogCoord, color); 
				
				//处理可能的另一种自定义雾效颜色选项
				//#ifdef UNITY_PASS_FORWARDADD
				//	UNITY_APPLY_FOG_COLOR(i.fogCoord, color, float4(0,0,0,0));
				//#else
				//	fixed4 myCustomColor = fixed4(0,0,1,0);
				//	UNITY_APPLY_FOG_COLOR(i.fogCoord, color, myCustomColor);
				//#endif
				
                return color;
            }
            ENDCG
        }
    }
}
```

可在此处下载上文中显示的示例（以 [Unity 项目压缩包](../uploads/Examples/UnityShaderDocExamples.zip)的形式提供）。

## 阅读更多信息

* [编写顶点和片元着色器程序](SL-ShaderPrograms.html)。
* [着色器语义](SL-ShaderSemantics.html)。
* [编写表面着色器](SL-SurfaceShaders.html)。
* [着色器参考](SL-Reference.html)。
