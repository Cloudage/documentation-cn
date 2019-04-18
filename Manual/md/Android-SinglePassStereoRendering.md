#Android 单通道立体渲染

Gear VR 和 Daydream 设备目前支持单通道立体渲染。要启用此功能，我们需要使用 [GL_OVR_multiview2](https://www.khronos.org/registry/OpenGL/extensions/OVR/OVR_multiview2.txt) 和 [GL_OVR_multiview_multisampled_render_to_texture](https://www.khronos.org/registry/OpenGL/extensions/OVR/OVR_multiview_multisampled_render_to_texture.txt) OpenGL|ES 扩展*。这些扩展需要使用具有 2 个切片（每只眼睛对应一个切片）的纹理 2D 数组。这与我们的 PC/PS4 实现方式不同，在 PC/PS4 实现方式中，我们只使用双宽 2D 纹理。因此，需要不同的着色器更改。

## 着色器脚本要求

您可能需要添加下面列出的额外代码，才能使用自定义着色器进行单通道立体渲染。这不适用于 Unity 会自动进行转换的表面着色器或固定函数管线着色器，但具有自定义顶点处理功能的表面着色器除外。

请注意，这些着色器脚本更改与多通道立体渲染兼容，因此在该模式下不会产生任何副作用。

首先，如果希望在顶点着色器之后的着色器阶段使用 `unity_StereoEyeIndex`，必须在所有着色器阶段输出结构中声明 `UNITY_VERTEX_OUTPUT_STEREO`。下面是一个示例：

```
struct v2f {
    float2 uv : TEXCOOR0;
    float4 vertex : SV_POSITION;
    UNITY_VERTEX_OUTPUT_STEREO
};
```

接下来，必须在顶点着色器函数中使用 `UNITY_INITIALIZE_VERTEX_OUTPUT_STEREO()` 来初始化输出数据：

```
v2f vert (appdata v)
{
	v2f o;
	UNITY_INITIALIZE_VERTEX_OUTPUT_STEREO(o);
    o.vertex = UnityObjectToClipPos(v.vertex);
    o.uv = TRANSFORM_TEX(v.uv, _MainTex);
    return o;
}
```

要在后续阶段初始化 `unity_StereoEyeIndex`，必须在开头添加 `UNITY_SETUP_STEREO_EYE_INDEX_POST_VERTEX()`，如下所示：

```
fixed4 frag (v2f i) : SV_Target
{
    UNITY_SETUP_STEREO_EYE_INDEX_POST_VERTEX(i);
	// 对纹理进行采样
	fixed4 col = tex2D(_MainTex, i.uv);
	// 应用雾
	UNITY_APPLY_FOG(i.fogCoord, col);
	return col;
}

```

如果正在使用其他着色器阶段，还应使用 `UNITY_TRANSFER_VERTEX_OUTPUT_STEREO()` 宏将眼睛索引传输到后续阶段。

最后，建议使用 `UnityObjectToClipPos(IN.vertex)` 而不是 mul`(UNITY_MATRIX_MVP, IN.vertex)` 来计算对象的最终位置。

#后期处理着色器

必须更新后期处理着色器以处理作为纹理 2D 数组的眼睛纹理。为了解决这一问题，我们创建了以下宏：`UNITY_DECLARE_SCREENSPACE_TEXTURE()`。应使用此宏来包装所有纹理，使纹理在多通道和单通道模式下均有效。

对纹理进行采样时，应使用以下宏：

`UNITY_SAMPLE_SCREENSPACE_TEXTURE()`

在单通模式下时，此宏要求预先调用 `UNITY_SETUP_STEREO_EYE_INDEX_POST_VERTEX()`。我们还为深度纹理和屏幕空间阴影贴图创建了类似的宏。您可以在 HLSLSupport.cginc 的底部看到完整列表。

请参阅[顶点和片元着色器示例](SL-VertexFragmentShaderExamples.html)页面以了解有关着色器代码的更多信息。

__* 这些 OpenGL|ES 扩展相对较新，因此很遗憾，可能会因为这些手机上的驱动程序而出现与图形相关的问题。__

---

* <span class="page-history">5.5 中的新功能</span>

*  <span class="page-edit">2017-06-20  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>
