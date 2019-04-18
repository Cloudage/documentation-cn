崩溃
=======


崩溃检查清单
---------------------



* 禁用代码剥离（并为 iOS 设置“slow with exceptions”）
* 按照“优化构建的 iOS 播放器的大小”(iphone-playerSizeOptimization)的说明，确保游戏不会在 iOS 上因代码剥离而崩溃。
* 确认崩溃不是因为内存不足（重新启动设备，将设备的最大 RAM 用于该平台，务必查看日志）

Editor 上的 Editor.log
--------------------------


调试消息、警告和错误都将输出到控制台。Unity 还会将状态报告输出到控制台：加载资源、初始化 Mono、图形驱动程序信息。

如果想了解目前发生的情况，请查看 editor.log。您将在该日志中看到所有内容，而非只有控制台部分。可尝试了解目前发生的情况，并查看编码会话的完整日志。这将有助于找出导致 Unity 崩溃的原因或弄清楚资源的问题。

Unity 还会在设备（Android 的 Logcat 控制台和 iOS 设备上的 Xcode gdb 控制台）上输出一些信息

Android 上的调试
--------------------

1.使用 _DDMS_ 或 _ADB_ 工具
1.查看堆栈跟踪（Android 3 或更高版本）。使用 _c++filt_（_ndk_ 的一部分）或其他方法（如：http://slush.warosu.org/c++filtjs）对损坏的函数调用进行解码
1.查看出现崩溃的 _.so_ 文件：
    1._libunity.so_ - 崩溃出现在 Unity 代码或用户代码中
    1._libdvm.so_ - 崩溃发生在 Java 世界的 Dalvik 某个位置。因此，请找到 Dalvik 的堆栈跟踪，查看 JNI 代码或任何与 Java 相关的内容（包括可能对 _AndroidManifest.xml_ 进行的更改）。
    1._libmono.so_ - Mono 错误或执行的操作不符合 Mono 的规则
1.如果崩溃日志没有帮助作用，可将日志进行分解以粗略了解发生的情况。
    1.使用 Android NDK 中的 ARM EABI 工具，如下所示：_objdump.exe -S libmono.so &gt;&gt; out.txt_
    1.从堆栈跟踪中查看 PC 相关代码。
    1.尝试在全新的 _out.txt_ 文件中匹配该代码。
    1.向上滚动以了解出现崩溃的函数中发生的情况。

iOS 上的调试
----------------

1.Xcode 具有内置的工具。Xcode 4 有一个非常友好的 GUI 可用于调试崩溃，Xcode 3 则稍逊一筹。
1.完整 gdb 堆栈 - thread backtrace all
1.启用 soft-null-check：
启用开发版本和脚本调试。现在，未捕获的 null 引用异常将通过适当的托管调用堆栈输出到 Xcode 控制台。

1.尝试关闭“快速脚本调用”和代码剥离。这样做可能会阻止一些随机崩溃，例如因使用一些罕见的 _.Net_ 函数或反射引起的崩溃。

策略
--------

1.尝试找出发生崩溃的脚本，并在设备上使用 MonoDevelop 对其进行调试。
1.如果崩溃看起来不在您的代码中，请仔细查看堆栈跟踪，应该能找到一些问题的线索。请复制一份并提交，让我们帮您诊断。
