Windows 调试
=================

Unity 提供了一些工具来简化 Windows 上的调试，从而方便进行游戏或 Editor 进程的取证或实时调试。

### 本机与托管调试
首先，关于调试的说明。Unity 中需要使用两种类型的调试。即：本机 C++ 调试以及 C# 托管调试。对于支持 IL2CPP 的平台，只会有本机调试，但为了实现快速迭代，托管调试将保留供 Editor 使用。

#### 本机调试
通过为相关的二进制文件（exe 或 dll）提供符号（pdb 文件），可以简化本机调试。

#### 托管调试
在 Windows 上，标准的 .NET 托管符号也存储在 pdb 文件中，但在使用 Mono 时，存在 mdb 文件。

### 符号
Unity 在 http://symbolserver.unity3d.com/ 上提供了一个符号存储库。此服务器 URL 可在 windbg 或 VS2012 及更高版本中用于自动进行符号解析和下载（很像 Microsoft 的符号存储库）。

#### Windbg 设置
在 windbg 上添加符号存储库的简单方法是使用 .sympath 命令。
> `.sympath+ SRV*c:\symbols-cache*http://symbolserver.unity3d.com/`

让我们对此进行分解说明：

> `.sympath+`  
+ 添加操作，仅保留现有的符号路径，并附加此符号存储库查找

> `SRV*c:\symbols-cache`  
SRV 表示用于获取符号的远程服务器，而 c:\symbols 是本地路径，用于缓存下载的符号并在再次下载之前先查看该位置。

> `*http://symbolserver.unity3d.com/`
用于获取符号的符号存储库的路径

#### Visual Studio 设置
**注意：**VS2010 和更早版本不支持 http 服务器符号存储库。
1.选择 Tools > Options
2.展开 Debugging Section，选择 Symbols
3.指定缓存目录（如果尚未指定）
4.添加 http://symbolserver.unity3d.com/ 作为“Symbol file (.pdb) location”
 
### 实时调试
实时调试是将调试器连接到正常运行的进程或连接到已捕获异常的进程的方案。为了让调试器了解发生的情况，需要将符号包含在构建中。这就是上面的步骤应该解决的问题。另外需要注意的是，游戏可执行文件是根据游戏名称命名的，因此如果调试器无权访问重命名后的可执行文件，则可能无法找到正确的 pdb。


#### 设置自动异常调试
在 Windows 上，Microsoft 自动设置为在应用程序崩溃时进入 Dr Watson/向 Microsoft 报告错误。但是，如果安装了 Visual Studio 或 windbg，精明的开发者可以充分利用 Microsoft 提供的[崩溃调试](https://msdn.microsoft.com/en-us/library/windows/desktop/bb204634(v=vs.85).aspx)工具。
为便于安装，可使用以下注册表文件内容进行安装：

    Windows Registry Editor Version 5.00
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AeDebug]
    "Auto"="1"
    [HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows NT\CurrentVersion\AeDebug]
     "Auto"="1"

有关 Editor 调试的一点额外信息：
> `Unity.exe -dbgbreak`  
将会启动 Unity 并立即提供要连接的调试器（如果设置了自动崩溃处理）

### 事后分析/取证调试
Windows 提供了调查故障转储文件（.dmp 或 .mdmp）的工具。根据故障转储的类型，故障转储可能仅仅是堆栈信息，也可能是整个进程内存。根据内容的不同，在了解可能导致崩溃的原因时存在各种可能性。在常规情况下，通常至少有一个堆栈可供调查（如果这是一个有效堆栈...）

要调查转储文件，可选择使用 Visual Studio 或 windbg 来加载该文件。虽然 Visual Studio 是更友好的工具，但其功能比 windbg 更有限。

### 调试提示和技巧

#### 在本机托管的异常
NullReferenceException 通常如下所示：

 	    1b45558c()	
 	    >	mono.dll!malloc(unsigned int size=12)  Line 163 + 0x5f bytes	C  
 	        mono.dll!g_hash_table_insert_replace(_GHashTable * hash=0x065c3960, void * key=0x0018cba4, void * value=0x0018cb8c, int replace=457528232)  Line 204 + 0x7 bytes	C  
 	    	mono.dll!mono_jit_runtime_invoke(_MonoMethod * method=0x242bf8b0, void * obj=0x065c3960, void ** params=0x0018cba4, MonoObject * * exc=0x0018cb8c)  Line 4889 + 0xc bytes	C
这不是 malloc 中的也不是 mono 中的崩溃，而是 NullReferenceException，符合以下条件之一：
* 由 VS 调试器捕获
* 在用户的播放器中未作处理，导致播放器退出

#### 查看托管堆栈帧

再次采用上一个示例：

 	    1b45558c()	
 	    >	mono.dll!malloc(unsigned int size=12)  Line 163 + 0x5f bytes	C  
 	        mono.dll!g_hash_table_insert_replace(_GHashTable * hash=0x065c3960, void * key=0x0018cba4, void * value=0x0018cb8c, int replace=457528232)  Line 204 + 0x7 bytes	C  
 	    	mono.dll!mono_jit_runtime_invoke(_MonoMethod * method=0x242bf8b0, void * obj=0x065c3960, void ** params=0x0018cba4, MonoObject * * exc=0x0018cb8c)  Line 4889 + 0xc bytes	C

不含任何信息的行即为托管帧。但是，有一种获取托管堆栈信息的途径：mono 有一个名为 mono_pmip 的内置函数，该函数接受堆栈帧的地址并返回附带信息的 char*。可在 Visual Studio 即时窗口中调用 mono_pmip：

> `?(char*)mono.dll!mono_pmip((void*)0x1b45558c)`  
0x26a296c0 " Tiles:OnPostRender () + 0x1e4 (1B4553A8 1B4555DC) [065C6BD0 - Unity Child Domain]"`

**注意：**这仅适用于 mono.dll 符号正确加载的情况。

#### 强制应用程序创建转储

有时，在某些情况下，应用程序连接了调试器但不崩溃，或者应用程序在远程设备上发生崩溃但调试器不可用。但是，如果可以获取转储文件，仍然能获得有用的信息；如需获取转储文件，请按照以下步骤操作。

**注意：**这些说明适用于 Windows 独立平台和通用 Windows 平台（在桌面端运行时）。

* 打开注册表。
* 找到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting
* 创建 LocalDumps 文件夹（如果不存在）。
* 添加以下注册表项：
    * "DumpFolder"=&lt;此处为文件夹路径&gt;，例如 C:\Temp
    * "DumpCount"=dword:00000010
    * "DumpType"=dword:00000002
* 启动应用程序（可以是 Windows 上运行的任何程序 - 通用 Windows 平台或 Windows 独立平台可执行文件）
* 重现崩溃，此时应该会在指定的文件夹中创建转储文件。可使用 Visual Studio 或 WinDbg 打开转储文件。

---

<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
