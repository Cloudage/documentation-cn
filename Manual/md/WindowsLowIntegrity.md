# Windows 低完整性

Windows 独立平台播放器可检测自身是否正在以低完整性级别运行（请参阅 Microsoft 有关[设计应用程序以低完整性级别运行 (Designing Applications to Run at a Low Integrity Level)](https://msdn.microsoft.com/en-us/library/bb625960.aspx) 的文档以了解更多信息）。在这种情况下会发生以下几件事：

* 日志文件写入到 %USER PROFILE%\AppData\LocalLow\\__CompanyName__\\__ProductName__
* PlayerPrefs 保存到 HKCU\Software\AppDataLow\Software\\__CompanyName__\\__ProductName__
