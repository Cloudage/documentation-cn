通用 Windows 平台：部署
=========================


###如何部署通用 Windows 应用程序



* 进入 Build Settings 并将项目构建到文件夹
* 将会生成 Visual Studio 解决方案


* 在**本地计算机**上运行：
    * 确保所选的解决方案平台为 x86 或 x64，并且 **Debug target** 设置为**“Local machine”**
    * 按 F5 或选择 **Debug** &gt; **Start debugging**


* 在**远程机器**上运行：
    * 如果要部署到 ARM 架构设备，请确保 **Solution Platform** 设置为 ARM
    * 在远程机器上启动 **Visual Studio Remote debugger**
    * 选择 Visual Studio **project settings** &gt; **Debug**
    * 将目标设备设置为**“Remote machine”**
    * 在列表中找到一台远程机器并选择“Universal”身份验证方法
    * 按 **F5** 或选择 **Debug** &gt; **Start debugging**
    * **注意：**如果无法连接到远程机器，请确保防火墙不会阻止该机器


* 在**模拟器**中运行：
    * 将 **Debug Target** 设置为**“Simulator”**
    * 按 F5 或选择 **Debug** &gt; **Start debugging**

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
