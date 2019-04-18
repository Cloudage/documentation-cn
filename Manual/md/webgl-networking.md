WebGL 网络
================

无法直接访问套接字
-----------------------

由于存在安全隐患，JavaScript 代码无法直接访问 IP 套接字来实现网络连接。因此，.NET 网络类（即 `System.Net` 命名空间中的所有内容，具体而言就是 `System.Net.Sockets`）在 WebGL 中不起作用。Unity 旧有的 `UnityEngine.Network*` 类也是如此，以 WebGL 为构建目标时无法使用这些类。

如果需要在 WebGL 中使用网络，当前可选择的做法是使用 Unity 中的 `WWW` 或 `UnityWebRequest` 类或使用支持 WebGL 的新型 [Unity 网络](UNetOverview.html)功能，或者使用 JavaScript 中的 WebSockets 或 WebRTC 来实现您自己的网络。

使用 WebGL 中的 WWW 或 WebRequest 类
--------------------------------------------

WebGL 支持 __[WWW](../ScriptReference/WWW.html)__ 和 __[UnityWebRequest](../ScriptReference/Networking.UnityWebRequest.html)__ 类。这些类是使用 JavaScript 中的 `XMLHttpRequest` 类实现的（使用浏览器来处理 WWW 请求）。这种情况下对访问跨域资源施加了一些安全限制。基本上，任何 WWW 请求若是发送到与托管 WebGL 内容的服务器不同的服务器，都需要由您尝试访问的服务器进行授权。为了在 WebGL 中访问跨域 WWW 资源，您尝试访问的服务器需要使用 CORS 对此访问进行授权。

如果您尝试使用 `WWW` 或 `UnityWebReqest` 来访问内容，但远程服务器未正确设置或配置 CORS，浏览器控制台中将显示如下错误：

````
Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at http://myserver.com/.This can be fixed by moving the resource to the same domain or enabling CORS.
````

CORS 表示跨源资源共享 (Cross-Origin Resource Sharing)，请参阅[此处](http://www.w3.org/TR/cors/)的说明。基本原理是，服务器需要向发出的 http 响应中添加一些 `Access-Control` 标头，从而告诉浏览器允许网页访问服务器上的内容。下面的标头设置示例将会允许 Unity WebGL 从任何源点访问 Web 服务器上的资源，此处采用了公共请求标头并使用 http `GET`、`POST` 或 `OPTIONS` 方法：

````
"Access-Control-Allow-Credentials": "true",
"Access-Control-Allow-Headers": "Accept, X-Access-Token, X-Application-Name, X-Request-Sent-Time",
"Access-Control-Allow-Methods": "GET, POST, OPTIONS",
"Access-Control-Allow-Origin": "*",
````

请注意，根据 CORS 规范 [7.1.1](http://www.w3.org/TR/cors/#handling-a-response-to-a-cross-origin-request)，WWW.responseHeaders 的范围仅限于实际响应标头的子集。

另外，请注意，XMLHttpRequest 不允许数据流，因此 WebGL 上的 WWW 类只会在下载完成后才处理数据（所以，和其他平台一样，在下载时不能对 AssestBundle 进行解压缩和加载）。

### 不要阻止 WWW 或 WebRequest 下载

不要使用如下所示的代码阻止 WWW 或 WebRequest 下载：

```
while(!www.isDone) {}
```

阻止 WWW 或 WebRequest 下载的做法在 Unity WebGL 上不起作用。因为 WebGL 采用单线程机制，并且由于 JavaScript 中的 `XMLHttpRequest` 类是异步的，所以除非您将控制权交回给浏览器，否则下载永远不会完成；相反，您的内容将发生死锁。取而代之的做法是使用[协程](Coroutines.html)和 `yield` 语句等待下载完成。

使用 Unity 网络
----------------------

[Unity 网络](UNetOverview.html)通过 WebSockets 协议实现通信，从而支持 WebGL。请参阅 __[Networking.NetworkManager.useWebSockets](../ScriptReference/Networking.NetworkManager-useWebSockets.html)__。

使用 JavaScript 中的 Web Sockets 或 WebRTC
-------------------------------------------

如上所述，在 WebGL 中无法直接访问 IP 套接字。但是，如果需要 WWW 类之外的网络功能，可使用 Web Sockets 或 WebRTC，这两种网络协议都受到浏览器的支持。Web Sockets 具有更广泛的支持，但 WebRTC 允许浏览器之间的 p2p 连接以及不可靠连接。这些协议均尚未通过 Unity 中的内置 API 公开，但可以使用 [JavaScript 插件](webgl-interactingwithbrowserscripting.html)来实现此目的。[AssetStore](https://www.assetstore.unity3d.com/en/#!/content/38367e) 上提供了一个在 Unity WebGL 中实现 WebSocket 网络的插件示例。
