#从 HTTP 服务器检索文本或二进制数据 (GET)

要从标准 HTTP 或 HTTPS Web 服务器检索简单数据（比如文本数据或二进制数据），请使用 `UnityWebRequest.GET` 调用。此函数将单个字符串作为参数，字符串用于指定从中检索数据的 URL。

此函数类似于标准 WWW 构造函数：

````
WWW myWww = new WWW("http://www.myserver.com/foo.txt");
// ...类似于 ...
UnityWebRequest myWr = UnityWebRequest.Get("http://www.myserver.com/foo.txt");
````

##详细信息

* 此函数将创建 ``UnityWebRequest`` 并将目标 URL 设置为字符串参数。此函数不会设置任何其他自定义标志或标头。
* 默认情况下，此函数将标准 ``DownloadHandlerBuffer`` 附加到 ``UnityWebRequest``。此处理程序可缓冲从服务器接收的数据，并在请求完成时将数据提供给脚本。
* 默认情况下，此函数不会将任何 ``UploadHandler`` 附加到 ``UnityWebRequest``。如果需要，可以手动附加。

##示例

````
using UnityEngine;
using System.Collections;
using UnityEngine.Networking;
 
public class MyBehaviour : MonoBehaviour {
    void Start() {
        StartCoroutine(GetText());
    }
 
    IEnumerator GetText() {
        UnityWebRequest www = UnityWebRequest.Get("http://www.my-server.com");
        yield return www.SendWebRequest();
 
        if(www.isNetworkError || www.isHttpError) {
            Debug.Log(www.error);
        }
        else {
            // 将结果显示为文本
            Debug.Log(www.downloadHandler.text);
 
            // 或者以二进制数据格式检索结果
            byte[] results = www.downloadHandler.data;
        }
    }
}
````
