#将原始数据上传到 HTTP 服务器 (PUT)

一些现代 Web 应用程序更喜欢通过 HTTP PUT 动词上传文件。针对这种情况，Unity 提供了 `UnityWebRequest.PUT` 函数。

此函数采用两个参数。第一个参数是一个字符串，用于指定请求的目标 URL。第二个参数是字符串或字节数组，用于指定要发送到服务器的有效负载数据。


函数签名：

````
WebRequest.Put(string url, string data);
WebRequest.Put(string url, byte[] data);
````

##详细信息

* 此函数创建 ``UnityWebRequest`` 并将内容类型设置为 ``application/octet-stream``。
* 此函数将标准 ``DownloadHandlerBuffer`` 附加到 ``UnityWebRequest``。与 POST 函数一样，此函数可用于从应用程序返回结果数据。
* 此函数将输入的上传数据存储在标准 ``UploadHandlerRaw`` 对象中，然后将此对象附加到 ``UnityWebRequest``。因此，如果使用 `byte[]` 函数，则在 ``UnityWebRequest.PUT`` 调用之后对字节数组执行的更改不会反映在上传到服务器的数据中。

##示例

```
using UnityEngine;
using UnityEngine.Networking;
using System.Collections;
 
public class MyBehavior : MonoBehaviour {
    void Start() {
        StartCoroutine(Upload());
    }
 
    IEnumerator Upload() {
        byte[] myData = System.Text.Encoding.UTF8.GetBytes("This is some test data");
        UnityWebRequest www = UnityWebRequest.Put("http://www.my-server.com/upload", myData);
        yield return www.SendWebRequest();
 
        if(www.isNetworkError || www.isHttpError) {
            Debug.Log(www.error);
        }
        else {
            Debug.Log("Upload complete!");
        }
    }
}
```
