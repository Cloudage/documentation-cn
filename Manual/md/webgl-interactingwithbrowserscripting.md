# WebGL：与浏览器脚本交互

构建适用于 Web 的内容时，可能需要与网页上的其他元素进行通信。或者，您可能希望使用 Unity 当前在默认情况下未公开的 Web API 来实现功能。在这两种情况下，都需要直接与浏览器的 JavaScript 引擎连接。Unity WebGL 提供了不同的方法来执行此操作。

## 从 Unity 脚本调用 JavaScript 函数

在项目中使用浏览器 JavaScript 的建议方法是将 JavaScript 源代码添加到项目中，然后直接从脚本代码中调用这些函数。为此，请使用 .jslib 扩展名（因为常规 .js 将由 UnityScript 编译器提取）将包含 JavaScript 代码的文件放置在 Assets 文件夹中的“Plugins”子文件夹下。插件文件需要有如下所示的语法：

```
mergeInto(LibraryManager.library, {

  Hello: function () {
	window.alert("Hello, world!");
  },

  HelloString: function (str) {
	window.alert(Pointer_stringify(str));
  },

  PrintFloatArray: function (array, size) {
	for(var i = 0; i < size; i++)
  	console.log(HEAPF32[(array >> 2) + i]);
  },

  AddNumbers: function (x, y) {
	return x + y;
  },

  StringReturnValueFunction: function () {
    var returnStr = "bla";
    var bufferSize = lengthBytesUTF8(returnStr) + 1;
    var buffer = _malloc(bufferSize);
    stringToUTF8(returnStr, buffer, bufferSize);
    return buffer;
  },

  BindWebGLTexture: function (texture) {
	GLctx.bindTexture(GLctx.TEXTURE_2D, GL.textures[texture]);
  },

});
```

然后，可从 C# 脚本调用这些函数，如下所示：

```
using UnityEngine;
using System.Runtime.InteropServices;

public class NewBehaviourScript : MonoBehaviour {

    [DllImport("__Internal")]
    private static extern void Hello();

    [DllImport("__Internal")]
    private static extern void HelloString(string str);

    [DllImport("__Internal")]
    private static extern void PrintFloatArray(float[] array, int size);

    [DllImport("__Internal")]
    private static extern int AddNumbers(int x, int y);

    [DllImport("__Internal")]
    private static extern string StringReturnValueFunction();

    [DllImport("__Internal")]
    private static extern void BindWebGLTexture(int texture);

    void Start() {
        Hello();
        
        HelloString("This is a string.");
        
        float[] myArray = new float[10];
        PrintFloatArray(myArray, myArray.Length);
        
        int result = AddNumbers(5, 7);
        Debug.Log(result);
        
        Debug.Log(StringReturnValueFunction());
        
        var texture = new Texture2D(0, 0, TextureFormat.ARGB32, false);
        BindWebGLTexture(texture.GetNativeTextureID());
    }
}
```

简单的数字类型可在函数参数中传递给 JavaScript，无需进行任何转换。其他数据类型将作为 emscripten 堆（实际上就是 JavaScript 中的一个大型数组）中的指针进行传递。对于字符串，可使用 `Pointer_stringify` helper 函数转换为 JavaScript 字符串。要返回字符串值，必须调用 `_malloc` 来分配一些内存，并调用 `stringToUTF8` helper 函数向其中写入 JavaScript 字符串。如果字符串是返回值，则 il2cpp 运行时将负责为您释放内存。对于原始类型的数组，`emscripten` 针对内存的不同大小的整数、无符号整数或浮点数表示形式，提供其堆的不同 `ArrayBufferViews`：__HEAP8、HEAPU8、HEAP16、HEAPU16、HEAP32、HEAPU32、HEAPF32、HEAPF64__。为了在 WebGL 中访问纹理，emscripten 提供了 `GL.textures` 数组，该数组将本机纹理 ID 从 Unity 映射到 WebGL 纹理对象。可在 emscripten 的 WebGL 上下文 `GLctx` 中调用 WebGL 函数。

有关如何与 JavaScript 交互的更多信息，请参阅 [emscripten 文档](https://kripken.github.io/emscripten-site/docs/porting/connecting_cpp_and_javascript/Interacting-with-code.html)。


另外，请注意，在 Unity 安装文件夹中有几个插件可供参考（位于 `PlaybackEngines/WebGLSupport/BuildTools/lib` 和 `PlaybackEngines/WebGLSupport/BuildTools/Emscripten/src/library*` 中）。

### 从 Unity 调用 JavaScript 代码的传统方法

*注意：从 Unity 5.6 开始，从 Unity 调用 JavaScript 代码的建议方法是通过 .jslib 插件。下述方法受到支持仅出于兼容性原因，并可能在 Unity 的未来版本中被弃用。*

可使用 [Application.ExternalCall()](../ScriptReference/Application.ExternalCall.html) 和 [Application.ExternalEval()](../ScriptReference/Application.ExternalEval.html) 函数在嵌入网页上调用 JavaScript 代码。请注意，表达式在构建的局部作用域内进行计算。如果希望在全局作用域内执行 JavaScript 代码，请参阅下面的*代码可见性*部分。

## 从 JavaScript 调用 Unity 脚本函数

有时需要从浏览器的 JavaScript 向 Unity 脚本发送一些数据或通知。建议的做法是调用内容中的游戏对象上的方法。如果要从嵌入在项目中的 JavaScript 插件执行调用，可使用以下代码：

  `SendMessage(objectName, methodName, value);`

其中，__objectName__ 是场景中的对象名称；__methodName__ 是当前附加到该对象的脚本中的方法名称；__value__ 可以是字符串、数字，也可为空。例如：

```
SendMessage('MyGameObject', 'MyFunction');
SendMessage('MyGameObject', 'MyFunction', 5);

SendMessage('MyGameObject', 'MyFunction', 'MyString');
```

如果希望从嵌入页面的全局作用域内执行调用，请参阅下面的*代码可见性*部分。

## 从 Unity 脚本调用 C++ 函数

由于 Unity 使用 emscripten 将源代码从 C++ 代码编译为 JavaScript，因此您也可以使用 C 或 C++ 代码编写插件，并从 C# 调用这些函数。因此，您可以不使用上面示例中的 jslib 文件，而是在项目中使用如下所示的 c 文件；它将自动使用您的脚本实现编译，并且您可以从中调用函数，就像上面的 JavaScript 示例一样。

如果使用 C++ (.cpp) 来实现该插件，则必须确保使用 C 链接来声明函数以免发生名称错用问题。

```
# include <stdio.h>
void Hello ()
{
    printf("Hello, world!\n");
}
int AddNumbers (int x, int y)
{
    return x + y;
}
```

### 代码可见性

从 Unity 5.6 开始，所有构建代码都在自己的作用域内执行。这种方法可以将游戏嵌入任意页面，而不会与嵌入页面代码发生冲突，并且可以在同一页面上嵌入多个构建版本。

如果项目中包含 *.jslib* 插件形式的所有 JavaScript 代码，则此 JavaScript 代码的运行作用域将与编译的构建相同，并且代码应该与 Unity 先前版本中的工作方式非常相似（例如，以下对象和函数应该在 JavaScript 插件代码中直接可见：*Module、SendMessage、HEAP8、ccall 等*）。

但是，如果您计划从嵌入页面的全局作用域中调用内部 JavaScript 函数，应始终假设该页面上嵌入了多个构建，因此应显式指定要引用的构建。例如，如果游戏已被实例化为：

`var gameInstance = UnityLoader.instantiate("gameContainer", "Build/build.json", {onProgress: UnityProgress});`

则可以使用 *gameInstance.SendMessage()* 向构建发送消息，或以类似 *gameInstance.Module* 的形式访问构建 Module 对象。

---

* <span class="page-edit">2017-11-14  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">修复了代码示例中的错误。</span>

* <span class="page-history">5.6 版更新</span>

