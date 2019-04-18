#Debugging and trouble shooting WebGL builds

Unity WebGL content cannot currently be debugged in Visual Studio, which can make it difficult to find out what exactly is going wrong with your content. Here are some tips on how to get information out of your build.

## The browser's JavaScript console

Unity WebGL does not have access to your file system, so it will not write a log file like other platforms. However, it will write all logging information which would normally go to the log file (such as `Debug.Log`, `Console.WriteLine` or Unity's internal logging) to the browser's JavaScript console.

* In Firefox, you can open the JavaScript console by pressing Ctrl-Shift-K on Windows or Command-Option-K on a Mac
* In Chrome, you can open the JavaScript console by pressing Ctrl-Shift-J on Windows or Command-Option-J on a Mac.
* In Safari, you can open the JavaScript console by enabling the Develop menu in the Advanced tabs in Preferences, and then pressing Command-Option-C.
* In Microsoft Edge or Internet Explorer, you can open the JavaScript console by pressing F12.

## Development builds

For debugging purposes, you may typically want to make a development build in Unity (the _Development Build_ checkbox in the [Build Settings window](PublishingBuilds.html)). Development builds allow you to connect the profiler, and they will not be [minified](https://en.wikipedia.org/wiki/Minification_%28programming%29), so the emitted JavaScript code will still contain human-readable (though [C++-mangled](https://en.wikipedia.org/wiki/Name_mangling#Name_mangling_in_C.2B.2B)) function names. These can be used by the browser to display stack traces when you run into a browser error, or when you throw an exception and exception support is disabled, or when using `Debug.LogError`. Unlike the managed stack traces which you can get when enabling Full exception support (see below), these stack traces will have mangled names, and contain not only managed code, but also the internal UnityEngine code.

## Exception support

WebGL has different levels of exception support (See the page on [Building for WebGL](webgl-building.html)). By default, Unity WebGL will only support explicitly thrown exceptions. You can enable _Full_ exception support which will then emit additional checks in the IL2CPP generated code to catch access to null references and out-of-bounds array elements in your managed code. These additional checks will significantly impact performance and increase code size and load times, so this mode is only recommended for debugging.

_Full_ exception support will also emit function names to generate stack traces for your managed code. So you will see stack traces in the console for uncaught exceptions and for `Debug.Log` statements, and you can get a stack trace string using `System.Environment.Stacktrace`.

---
* <span class="page-edit">2018-03-19  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">从 2018.1 开始，MonoDevelop 由 Visual Studio 取代</span>
