#创建 UploadHandler

目前，仅一种类型的上传处理程序可用：`UploadHandlerRaw`。此类在构建时接受数据缓冲区。此缓冲区在内部复制到本机代码内存中，然后当远程服务器准备好接受主体数据时，此缓冲区由 `UnityWebRequest` 系统使用。

上传处理程序还接受“Content Type”字符串。如果在 UnityWebRequest 自身中没有设置 `Content-Type` 标头，则此字符串用于 UnityWebRequest 的 `Content-Type` 标头值。如果在 UnityWebRequest 对象上手动设置了 `Content-Type` 标头，则将忽略上传处理程序对象上的 `Content-Type`。

如果没有在 UnityWebRequest 或 `UploadHandler` 上设置 `Content-Type`，则系统会将 `Content-Type` 默认设置为 `application/octet-stream`。

`UnityWebRequest` 具有一个 `disposeUploadHandlerOnDispose` 属性，其默认值为 true。如果此属性为 true，则在处理 UnityWebRequest 对象时还将在附加的上传处理程序上调用 Dispose()，致使下载处理程序无用。如果保持对上传处理程序的引用时间长度超过对 UnityWebRequest 的引用时间长度，应将 disposeUploadHandlerOnDispose 设置为 false。

##示例

````
byte[] payload = new byte[1024];
// ...使用数据填充有效负载 ...

UnityWebRequest wr = new UnityWebRequest("http://www.mysite.com/data-upload");
UploadHandler uploader = new UploadHandlerRaw(payload);

// 发送标头："Content-Type: custom/content-type";
uploader.contentType = "custom/content-type";

wr.uploadHandler = uploader;
````
