#从 HTTP 服务器检索纹理 (GET)

要从远程服务器检索纹理文件，可使用 `UnityWebRequest.Texture.`。此函数与 `UnityWebRequest.GET` 非常类似，但进行了优化，可高效下载和存储纹理。

此函数采用单个字符串作为参数。此字符串指定要下载图像文件（以用作纹理）的 URL。

##详细信息

* 此函数将创建 `UnityWebRequest` 并将目标 URL 设置为字符串参数。此函数不会设置任何其他标志或自定义标头。
* 此函数将 `DownloadHandlerTexture` 对象附加到 `UnityWebRequest`。DownloadHandlerTexture 是一个进行了优化的专用下载处理程序，用于存储要在 Unity 引擎中用作纹理的图像。与下载原始字节并在脚本中手动创建纹理相比，使用此类可显著减少内存重新分配。
* 默认情况下，此函数不会附加上传处理程序。如果需要，可以手动添加。

##示例

````
using UnityEngine;
using System.Collections;
using UnityEngine.Networking;
 
public class MyBehaviour : MonoBehaviour {
    void Start() {
        StartCoroutine(GetTexture());
    }
 
    IEnumerator GetTexture() {
        UnityWebRequest www = UnityWebRequestTexture.GetTexture("http://www.my-server.com/image.png");
        yield return www.SendWebRequest();

        if(www.isNetworkError || www.isHttpError) {
            Debug.Log(www.error);
        }
        else {
            Texture myTexture = ((DownloadHandlerTexture)www.downloadHandler).texture;
        }
    }
}
````

或者也可以使用 helper getter 来实现 GetTexture：

````
    IEnumerator GetTexture() {
            UnityWebRequest www = UnityWebRequestTexture.GetTexture("http://www.my-server.com/image.png");
            yield return www.SendWebRequest();

            Texture myTexture = DownloadHandlerTexture.GetContent(www);
        }
````
