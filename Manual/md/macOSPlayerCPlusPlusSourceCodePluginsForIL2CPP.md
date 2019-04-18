# macOS 播放器：适用于 IL2CPP 的 C++ 源代码插件

使用 [IL2CPP](IL2CPP.html) 脚本后端时，可将 C++ (.cpp) 代码文件直接添加到 Unity 项目中。这些 C++ 文件将充当 Plugin Inspector 中的插件。如果将 C++ 文件配置为与 macOS 播放器兼容，则 Unity 会将这些文件与从托管程序集生成的 C++ 代码一起编译。在 Inspector 窗口的 __Platform settings__ 部分中选择适当的 __Mac OS__ 选项：

![关于 C++ 文件的插件导入器设置](../uploads/Main/PlatformIL2CPPPlatformSettings.png)

因为函数与生成的 C++ 代码链接在一起，所以没有单独的 DLL 可进行 `_P/Invoke` 调用。因此，可使用`“__Internal”`关键字代替 DLL 名称，从而使 C++ 链接器负责解析函数，而不是在运行时加载函数，如下例所示：

```
[DllImport("__Internal")]
private static extern int
CountLettersInString([MarshalAs(UnmanagedType.LPWSTR)]string str);
```

可在 NativeFunctions.cpp 中定义此类函数，如下所示：

```
extern "C" __declspec(dllexport) int __stdcall CountLettersInString(wchar_t* str)
{
    int length = 0;
    while (*str++ != nullptr)
        length++;
    return length;
}
```

因为由链接器负责解析函数调用，所以在托管端的函数声明（即在托管运行时执行的 C# 代码）中发生的任何错误都会生成链接器错误而不是运行时错误。这也意味着，在运行时不需要进行动态加载，而直接从 C# 调用函数。这种方式显著降低了 `P/Invoke` 调用的性能开销。

## 另请参阅

[Plugin Inspector](PluginInspector.html)



* <span class="page-edit">• 2018-03-13  Page published with [editorial review](DocumentationEditorialReview.html)
</span><br/>

* <span class="page-history">[2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 中的新功能 <span class="search-words">NewIn20181</span></span>
