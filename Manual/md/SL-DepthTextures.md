# 使用深度纹理


可创建[渲染纹理](class-RenderTexture.html)来使每个像素包含一个高精度[深度 (Depth)](../ScriptReference/RenderTextureFormat.Depth.html) 值。这主要用于某些效果需要使用场景深度的情况（例如，软粒子、屏幕空间环境光遮挡和半透明全都需要场景深度）。[图像效果](PostProcessingWritingEffects.html)也经常使用深度纹理。

深度纹理中的像素值介于 0 和 1 之间，具有非线性分布。精度通常为 32 或 16 位，具体取决于所使用的配置和平台。从深度纹理读取时，将返回 0 到 1范围内的高精度值。如果您需要获取与摄像机之间的距离或其他 0 到 1 之间的线性值，请使用 helper 宏手动进行计算（见下文）。

大多数现代硬件和图形 API 都支持深度纹理。下面列出了特殊要求：

* Direct3D 11+ (Windows)、OpenGL 3+ (Mac/Linux)、OpenGL ES 3.0+ (Android/iOS)、Metal (iOS) 和游戏主机（如 PS4/Xbox One）全都支持深度纹理。
* OpenGL ES 2.0 (iOS/Android) 要求存在 [GL_OES_depth_texture](http://www.khronos.org/registry/gles/extensions/OES/OES_depth_texture.txt) 扩展。
* WebGL 要求存在 [WEBGL_depth_texture](https://www.khronos.org/registry/webgl/extensions/WEBGL_depth_texture) 扩展。



## 深度纹理着色器 helper 宏


大多数情况下，深度纹理用于渲染距离摄像机的深度。在此情况下，[UnityCG.cginc include 文件](SL-BuiltinIncludes.html)包含的一些宏可以处理上述复杂问题：

* __UNITY_TRANSFER_DEPTH(o)__：计算顶点的眼睛空间深度并将其在 __o__ 中输出（必须是 float2）。当渲染到深度纹理时，在顶点程序中使用此宏。在具有本机深度纹理的平台上，此宏完全不执行任何操作，因为 Z 缓冲区值是隐式渲染的。
* __UNITY_OUTPUT_DEPTH(i)__：从 __i__ 返回眼睛空间深度（必须为 float2）。当渲染到深度纹理时，在片元程序中使用此宏。在具有本机深度纹理的平台上，此宏始终返回零，因为 Z 缓冲区值是隐式渲染的。
* __COMPUTE_EYEDEPTH(i)__：计算顶点的眼睛空间深度并将其在 __o__ 中输出。当__不__渲染到深度纹理时，在顶点程序中使用此宏。
* __DECODE_EYEDEPTH(i)/LinearEyeDepth(i)__：通过深度纹理 __i__ 给出高精度值时，返回相应的眼睛空间深度。
* __Linear01Depth(i)__：通过深度纹理 __i__ 给出高精度值时，返回相应的线性深度，范围在 0 到 1 之间。

__注意：__在 DX11/12、PS4、XboxOne 和 Metal 中，Z 缓冲区范围是 1 到 0，并定义了 UNITY_REVERSED_Z。在其他平台上，范围是 0 到 1。

例如，以下着色器将渲染其游戏对象的深度：

````
Shader "Render Depth" {
	SubShader {
	    Tags { "RenderType"="Opaque" }
	    Pass {
			CGPROGRAM

			#pragma vertex vert
			#pragma fragment frag
			#include "UnityCG.cginc"

			struct v2f {
			    float4 pos : SV_POSITION;
			    float2 depth : TEXCOORD0;
			};

			v2f vert (appdata_base v) {
			    v2f o;
			    o.pos = UnityObjectToClipPos(v.vertex);
			    UNITY_TRANSFER_DEPTH(o.depth);
			    return o;
			}

			half4 frag(v2f i) : SV_Target {
			    UNITY_OUTPUT_DEPTH(i.depth);
			}
			ENDCG
	    }
	}
}
````

## 另请参阅

* [摄像机深度纹理](SL-CameraDepthTexture.html)
* [编写图像效果](PostProcessingWritingEffects.html)
