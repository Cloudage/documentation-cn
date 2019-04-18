#纹理数组

类似于常规 2D 纹理（[Texture2D](class-TextureImporter.html) 类，着色器中的 __sampler2D__）、立方体贴图（[Cubemap](class-Cubemap.html) 类，着色器中的 __samplerCUBE__）和 3D 纹理（[Texture3D](class-Texture3D.html) 类，着色器中的 __sampler3D__），Unity 也支持 2D 纹理数组。

纹理数组是具有相同大小/格式/标记的 2D 纹理的集合，这些纹理对于 GPU 而言像是单个对象，并可在着色器中使用纹理元素索引进行采样。它们可以用于实现自定义地形渲染系统或其他特殊效果，让您高效访问大量相同大小和格式的纹理。2D 纹理数组的元素也称为切片或图层。

##平台支持

纹理数组需要受到底层图形 API 和 GPU 的支持。纹理数组在以下平台上可用：

* Direct3D 11/12（Windows、Xbox One）
* OpenGL Core（Mac OS X、Linux）
* Metal（iOS、Mac OS X）
* OpenGL ES 3.0（Android、iOS、WebGL 2.0）
* PlayStation 4

其他平台（OpenGL ES 2.0 或 WebGL 1.0）不支持纹理数组。可使用 [SystemInfo.supports2DArrayTextures](../ScriptReference/SystemInfo-supports2DArrayTextures.html) 在运行时确定纹理数组支持情况。

##创建和填充纹理数组

由于纹理数组没有纹理导入管线，必须在脚本中创建纹理数组。可使用 [Texture2DArray](../ScriptReference/Texture2DArray.html) 类来创建和填充纹理数组。请注意，纹理数组可序列化为资源，因此可以借助 Editor 脚本中的数据创建和填充纹理数组。

通常，纹理数组完全是在 GPU 内存中使用，但您可以使用 [Graphics.CopyTexture](../ScriptReference/Graphics.CopyTexture.html)、[Texture2DArray.GetPixels](../ScriptReference/Texture2DArray.GetPixels.html) 和 [Texture2DArray.SetPixels](../ScriptReference/Texture2DArray.SetPixels.html) 与系统内存之间双向传输像素。

##将纹理数组用作渲染目标
纹理数组元素也可用作渲染目标。使用 [RenderTexture.dimension](../ScriptReference/RenderTexture-dimension.html) 提前指定渲染目标是否是 2D 纹理数组。[Graphics.SetRenderTarget](../ScriptReference/Graphics.SetRenderTarget.html) 的 __depthSlice__ 参数可指定要渲染到的 Mipmap 级别或立方体贴图面。在支持“分层渲染”（例如，几何着色器）的平台上，可将 __depthSlice__ 参数设置为 -1 以便将整个纹理数组设置为渲染目标。此外还可使用几何着色器来渲染到个别元素中。

##在着色器中使用纹理数组
由于纹理数组并非适用于所有平台，因此着色器需要使用适当的[编译目标](SL-ShaderCompileTargets.html)或功能要求来访问纹理数组。支持纹理数组的最低着色器模型编译目标为 `3.5`，功能名称为 `2darray`。

使用以下[宏](SL-BuiltinMacros.html)可声明和采样纹理数组：

* UNITY_DECLARE_TEX2DARRAY(name) 在 HLSL 代码中声明纹理数组采样器变量。
* UNITY_SAMPLE_TEX2DARRAY(name,uv) 使用 __float3__ UV 采样纹理数组；坐标的 z 分量是数组元素索引。
* UNITY_SAMPLE_TEX2DARRAY_LOD(name,uv,lod) 使用显式 Mipmap 级别采样纹理数组。

##示例
以下着色器示例通过使用对象空间顶点位置作为坐标来采样纹理数组：

````
Shader "Example/Sample2DArrayTexture"
{
    Properties
    {
        _MyArr ("Tex", 2DArray) = "" {}
        _SliceRange ("Slices", Range(0,16)) = 6
        _UVScale ("UVScale", Float) = 1.0
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            // 纹理数组并非在任何地方都可用，
            // 只能在它们所在的平台上编译着色器
            #pragma require 2darray
            
            #include "UnityCG.cginc"

            struct v2f
            {
                float3 uv : TEXCOORD0;
                float4 vertex : SV_POSITION;
            };

            float _SliceRange;
            float _UVScale;

            v2f vert (float4 vertex : POSITION)
            {
                v2f o;
                o.vertex = mul(UNITY_MATRIX_MVP, vertex);
                o.uv.xy = (vertex.xy + 0.5) * _UVScale;
                o.uv.z = (vertex.z + 0.5) * _SliceRange;
                return o;
            }
            
            UNITY_DECLARE_TEX2DARRAY(_MyArr);

            half4 frag (v2f i) : SV_Target
            {
                return UNITY_SAMPLE_TEX2DARRAY(_MyArr, i.uv);
            }
            ENDCG
        }
    }
}
````


##另请参阅
* Direct3D 文档中的[纹理简介 (Introduction To Textures)](https://msdn.microsoft.com/en-us/library/windows/desktop/ff476906.aspx#Texture2D_Resource)。
* OpenGL Wiki 中的[数组纹理 (Array Textures)](https://www.opengl.org/wiki/Array_Texture)。

---

* <span class="page-edit">2018-03-20  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>
* <span class="page-history">在 Unity 2018.1 中添加了着色器 #pragma 指令</span>
