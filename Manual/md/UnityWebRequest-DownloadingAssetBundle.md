#从 HTTP 服务器下载 AssetBundle (GET)

要从远程服务器下载 AssetBundle，可使用 `UnityWebRequest.GetAssetBundle`。此功能将数据串流到内部缓冲区，后者负责在工作线程上解码和解压缩 AssetBundle 的数据。

此函数的参数有多种形式。最简单的形式仅采用一个参数：下载 AssetBundle 时应使用的 URL。您可以选择提供校验和来验证下载数据的完整性。

或者，如果希望使用 AssetBundle 缓存系统，可提供版本号或 Hash128 数据结构。这些值与通过 `WWW.LoadFromCacheOrDownload` 为旧系统提供的版本号或 `Hash128 objects` 相同。

##详细信息

* 此函数将创建 `UnityWebRequest` 并将目标 URL 设置为提供的 URL 参数。此函数还会将 HTTP 动词设置为 `GET`，但不会设置任何其他标志或自定义标头。
* 此函数将 `DownloadHandlerAssetBundle` 附加到 `UnityWebRequest`。此下载处理程序有一个特殊的 `assetBundle` 属性，一旦下载和解码了足够的数据，便可使用该属性来提取 AssetBundle，从而允许访问 AssetBundle 中的资源。
* 如果提供版本号或 `Hash128` 对象作为参数，也会将这些参数传递给 `DownloadHandlerAssetBundle`。下载处理程序随后将采用缓存系统。

##示例

````
using UnityEngine;
using UnityEngine.Networking;
using System.Collections;
 
public class MyBehaviour : MonoBehaviour {
    void Start() {
        StartCoroutine(GetAssetBundle());
    }
 
    IEnumerator GetAssetBundle() {
        UnityWebRequest www = UnityWebRequest.GetAssetBundle("http://www.my-server.com/myData.unity3d");
        yield return www.SendWebRequest();
 
        if(www.isNetworkError || www.isHttpError) {
            Debug.Log(www.error);
        }
        else {
            AssetBundle bundle = DownloadHandlerAssetBundle.GetContent(www);
        }
    }
}
````
