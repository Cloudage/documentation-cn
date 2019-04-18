#UnityWebRequest

UnityWebRequest 提供了一个模块化系统，用于构成 HTTP 请求和处理 HTTP 响应。UnityWebRequest 系统的主要目标是让 Unity 游戏与 Web 浏览器后端进行交互。该系统还支持高需求功能，例如分块 HTTP 请求、流式 POST/PUT 操作以及对 HTTP 标头和动词的完全控制。

该系统由两层组成：

* [高级 API](UnityWebRequest-HLAPI.html) (HLAPI) 封装了低级 API，并提供方便的界面来执行常见操作
* [低级 API](UnityWebRequest-LLAPI.html) (LLAPI) 为更高级的用户提供了最大的灵活性

##支持的平台

UnityWebRequest 系统支持大多数 Unity 平台：

* 所有版本的 Editor 和独立平台播放器
* WebGL
* 移动平台：iOS 和 Android
* 通用 Windows 平台
* PS4 和 PSVita
* XboxOne
* Nintendo Switch

##架构

UnityWebRequest 生态系统将 HTTP 事务分解为三个不同的操作：

* 向服务器提供数据
* 从服务器接收数据
* HTTP 流量控制（例如，重定向和错误处理）

为了给高级用户提供更好的界面，这些操作均由自己的对象进行管理：

* `UploadHandler` 对象处理数据到服务器的传输
* `DownloadHandler` 对象处理从服务器接收的数据的接收、缓冲和后处理
* `UnityWebRequest` 对象管理其他两个对象，还处理 HTTP 流量控制。此对象是定义自定义标头和 URL 的位置，也是存储错误和重定向信息的位置。

![](../uploads/Main/UnityWebRequestArchitecture.png) 

对于任何 HTTP 事务，正常的代码流程如下：

* 创建 Web 请求对象
* 配置 Web 请求对象
    * 设置自定义标头
    * 设置 HTTP 动词（例如 GET、POST 和 HEAD - 除 Android 之外的所有平台都允许使用自定义动词）
    * 设置 URL
*（可选）创建上传处理程序并将其附加到 Web 请求
    * 提供要上传的数据
    * 提供要上传的 HTTP 表单
*（可选）创建下载处理程序并将其附加到 Web 请求
* 发送 Web 请求
    * 如果在协程中，可获得 ``Send()`` 调用的结果以等待请求完成
*（可选）读取从下载处理程序接收的数据
*（可选）从 UnityWebRequest 对象中读取错误信息、HTTP 状态码和响应标头

---

<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
