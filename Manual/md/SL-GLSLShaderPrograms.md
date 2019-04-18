#GLSL 着色器程序

除了使用 [Cg/HSL 着色器程序](SL-ShaderPrograms.html)之外，还可以直接编写 OpenGL 着色语言 (GLSL) 着色器。

但是，只有需要进行测试时，或者当您知道目标平台仅限于 Mac OS X、OpenGL ES 移动设备或 Linux 时，才建议使用原始 GLSL。在所有正常情况下，Unity will 会在需要时将 Cg/HLSL 交叉编译为优化的 GLSL。


##GLSL 代码片段

在 `GLSLPROGRAM` 和 `ENDGLSL` 关键字之间编写 GLSL 程序代码片段。

在 GLSL 中，所有着色器函数入口点都必须名为 `main()`。Unity 加载 GLSL 着色器时，会为顶点程序加载一次源代码（使用 `VERTEX` 预处理器定义），然后为片元程序再加载一次（使用 `FRAGMENT` 预处理器定义）。所以，在 GLSL 代码片段中将顶点程序和片元程序部分分开的方法是在它们周围加上 `#ifdef VERTEX ..#endif` 和 #ifdef FRAGMENT ..#endif`。每个 GLSL 代码片段必须同时包含顶点程序和片元程序。

标准 include 文件与 Cg/HLSL 着色器的 include 文件匹配；只是它们的扩展名为 `.glslinc`：

````
UnityCG.glslinc
````

顶点着色器输入来自预定义的 GLSL 变量（`gl_Vertex`、`gl_MultiTexCoord0` 等等）或者是用户定义的属性。通常，只有切线矢量才需要用户定义的属性：

````
attribute vec4 Tangent;
````

从顶点程序到片元程序的数据是通过 _varying_ 变量传递的，例如：

````
varying vec3 lightDir; // 顶点着色器负责计算，由片元着色器使用
````

###外部 OES 纹理

Unity 在着色器编译期间会进行某些预处理；例如，根据图形 API（GlES3 或 GLCore），`texture2D/texture2DProj` 函数可能会替换为 `texture/textureProj`。一些扩展不支持新约定，最明显的就是 [`GL_OES_EGL_image_external`](https://www.khronos.org/registry/gles/extensions/OES/OES_EGL_image_external.txt)。

如果希望在 GLSL 着色器中采样外部纹理，请使用 `textureExternal/textureProjExternal` 调用，而不是 `texture2D/texture2DProj`。

示例：

````
gl_FragData[0] = textureExternal(_MainTex, uv);
````
