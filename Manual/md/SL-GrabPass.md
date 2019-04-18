# ShaderLab：GrabPass

GrabPass 是一种特殊通道类型，它把即将绘制对象时的屏幕内容抓取到纹理中。在后续通道中即可使用此纹理，从而执行基于图像的高级效果。

## 语法

GrabPass 包含在[子着色器](SL-SubShader.html)内部。它可采用两种形式：

* 简单的 `GrabPass { }` 可将当前屏幕内容抓取到某个纹理中。在随后的通道中可通过 `_GrabTexture` 名称访问该纹理。注意：这种抓取通道的形式将为使用它的每个对象执行耗时的屏幕抓取操作。
* `GrabPass { "TextureName" }` 可将当前屏幕内容抓取到纹理中，但仅为使用给定纹理名称的第一个对象在每一帧执行一次该操作。在后续通道中可通过给定纹理名称访问该纹理。场景中有多个对象在使用 GrabPass 时，这种方法更高效。

此外，GrabPass 还可以使用 [Name](SL-Name.html) 和 [Tags](SL-PassTags.html) 命令。


## 示例


以下是反转先前渲染内容的颜色的一种低效方法：


````
Shader "GrabPassInvert"
{
    SubShader
    {
        // 在所有不透明几何体之后绘制自己
        Tags { "Queue" = "Transparent" }

        // 将对象后面的屏幕抓取到 _BackgroundTexture 中
        GrabPass
        {
            "_BackgroundTexture"
        }

        // 使用上面生成的纹理渲染对象，并反转颜色
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            struct v2f
            {
                float4 grabPos : TEXCOORD0;
                float4 pos : SV_POSITION;
            };

            v2f vert(appdata_base v) {
                v2f o;
                // 使用 UnityCG.cginc 中的 UnityObjectToClipPos 来计算
                // 顶点的裁剪空间
                o.pos = UnityObjectToClipPos(v.vertex);
                // 使用 UnityCG.cginc 中的 ComputeGrabScreenPos 函数
                // 获得正确的纹理坐标
                o.grabPos = ComputeGrabScreenPos(o.pos);
                return o;
            }

            sampler2D _BackgroundTexture;

            half4 frag(v2f i) : SV_Target
            {
                half4 bgcolor = tex2Dproj(_BackgroundTexture, i.grabPos);
                return 1 - bgcolor;
            }
            ENDCG
        }

    }
}
````


此着色器有两个通道：第一个通道抓取渲染时对象后面的任何内容，然后在第二个通道中应用这些内容。请注意，可使用反转[混合模式](SL-Blend.html)更高效地实现相同效果。


## 另请参阅


* [常规 Pass 命令](SL-Pass.html)。
* [平台差异](SL-PlatformDifferences.html)。
* [内置着色器 helper 函数](SL-BuiltinFunctions.html)。
