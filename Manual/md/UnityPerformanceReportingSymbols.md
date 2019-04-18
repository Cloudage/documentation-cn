# Performance Reporting 缺失符号

符号用于将程序地址映射到函数名称。这些符号可让 Performance Reporting 服务提供带有可读函数名称的原生崩溃堆栈跟踪，而不仅仅是内存地址。这些符号可以包含在可执行文件中，但通常存储在单独文件中以减小可执行文件的大小，并增加篡改可执行文件的难度。

符号文件因操作系统不同而具有不同的格式。Apple 平台使用 dSYM 文件夹，Android 符号存储在 .so 文件中，Windows 符号存储在 .pdb 文件中。Performance Reporting 服务可以识别和处理所有这些格式。

Performance Reporting 服务使用两组不同的符号：

* 系统符号 - 系统符号由操作系统供应商生成。Unity 支持 Apple、Google 和 Microsoft 生成的符号。
* 应用程序符号 - 在编译 Unity 项目时生成应用程序符号。

## 为什么我的符号缺失？

符号文件具有必须与可执行文件的 ID 完全匹配的通用唯一标识符 (UUID) 或全局唯一标识符 (GUID)。如果 Performance Reporting 服务无法加载 ID 与库或模块匹配的符号文件，则会生成以下错误：

*&lt;system symbols missing&gt;*

*&lt;symbols missing for uuid: xxx...&gt;*

Performance Reporting 服务缺失符号文件会生成报告，报告显示在 Unity Services Dashboard 的 Performance 选项卡下的 Reports 部分。

## 缺失系统符号

缺失系统符号的报告包含堆栈跟踪中的一行（位于 Dashboard 的 Reports 部分）：

*&lt;symbols missing for uuid: xxx...&gt;*

通常，问题的原因是 Unity 没有该操作系统版本的符号。对于 iOS 和其他 Apple 平台，可能难以获取旧版操作系统的符号。Unity 的系统符号覆盖了从 iOS 7 系列开始的 iOS 版本的大约 80%。新的 iOS 版本一经发布，Unity 便会尽力获取操作系统的符号。

如果遇到此情况，可以检查是否有另一个操作系统版本的类似崩溃报告。如果确实有，也许可以通过在该版本的操作系统中进行调试来解决问题。

## 缺失应用程序符号

缺失应用程序符号的报告包含堆栈跟踪中的一行：

*&lt;symbols missing for uuid: xxx...&gt;*

编译启用了 Performance Reporting 的项目时，Unity 会生成一个符号文件并将其上传到 Performance Reporting 服务器。如果该过程失败，则符号缺失消息将显示在 Services Dashboard 中。

要排除符号上传中的故障，请检查与 [Unity 主日志](LogFiles.html)位于同一文件夹中的 `symbol_upload.log` 文件。该文件应该会显示找到并处理了哪些符号，以及在处理和上传符号过程中发生的任何错误。
