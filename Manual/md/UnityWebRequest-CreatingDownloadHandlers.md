#创建 DownloadHandler

`DownloadHandlers` 有多种类型：

* `DownloadHandlerBuffer` 用于简单的数据存储。
* `DownloadHandlerFile` 用于下载文件并将文件保存到磁盘（内存占用少）。
* `DownloadHandlerTexture` 用于下载图像。
* `DownloadHandlerAssetBundle` 用于提取 AssetBundle。
* `DownloadHandlerAudioClip` 用于下载音频文件。
* `DownloadHandlerMovieTexture` 用于下载视频文件。
* `DownloadHandlerScript` 是一个特殊类。就其本身而言，不会执行任何操作。但是，此类可由用户定义的类继承。此类接收来自 UnityWebRequest 系统的回调，然后可以使用这些回调在数据从网络到达时执行完全自定义的数据处理。

这些 API 类似于 `DownloadHandlerTexture` 的接口。

`UnityWebRequest` 具有一个 `disposeDownloadHandlerOnDispose` 属性，其默认值为 true。如果此属性为 true，则在处理 UnityWebRequest 对象时还将在附加的下载处理程序上调用 Dispose()，致使下载处理程序无用。如果保持对下载处理程序的引用时间长度超过对 UnityWebRequest 的引用时间长度，应将 disposeDownloadHandlerOnDispose 设置为 false。

##DownloadHandlerBuffer
此下载处理程序是最简单的，可处理大多数用例。它将接收的数据存储在本机代码缓冲区中。下载完成后，可使用字节数组或文本字符串的形式访问缓冲的数据。

###示例

````
using UnityEngine;
using UnityEngine.Networking; 
using System.Collections;

 
public class MyBehaviour : MonoBehaviour {
    void Start() {
        StartCoroutine(GetText());
    }
 
    IEnumerator GetText() {
        UnityWebRequest www = new UnityWebRequest("http://www.my-server.com");
        www.downloadHandler = new DownloadHandlerBuffer();
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

##DownloadHandlerFile
这是大型文件的特殊下载处理程序。它将下载的字节直接写入文件，因此无论下载的文件大小如何，内存使用量都很低。与其他下载处理程序的区别在于，无法从此下载处理程序获取数据，所有数据都将保存到文件中。


##示例
````
using System.Collections;
using System.IO;
using UnityEngine;
using UnityEngine.Networking;

public class FileDownloader : MonoBehaviour {

	void Start () {
		StartCoroutine(DownloadFile());
	}
	
	IEnumerator DownloadFile() {
        var uwr = new UnityWebRequest("https://unity3d.com/", UnityWebRequest.kHttpVerbGET);
        string path = Path.Combine(Application.persistentDataPath, "unity3d.html");
        uwr.downloadHandler = new DownloadHandlerFile(path);
        yield return uwr.SendWebRequest();
        if (uwr.isNetworkError || uwr.isHttpError)
            Debug.LogError(uwr.error);
        else
            Debug.Log("File successfully downloaded and saved to " + path);
    }
}
````

##DownloadHandlerTexture
相较于使用 ``DownloadHandlerBuffer`` 下载图像文件，然后再使用 ``Texture.LoadImage`` 从原始字节创建纹理，使用 ``DownloadHandlerTexture`` 会更有效。

此下载处理程序将接收的数据存储在 `UnityEngine.Texture`中。在下载完成时，它将 JPEG 和 PNG 解码为有效的 `UnityEngine.Texture objects`。每个 `DownloadHandlerTexture` 对象只创建一个 `UnityEngine.Texture` 副本。这样会减少垃圾收集的性能问题。此处理程序在本机代码中执行缓冲、解压缩和纹理创建。另外，解压缩和纹理创建任务在工作程序线程上而不是主线程上执行，这样可以在加载大型纹理时改善帧时间。

最后，``DownloadHandlerTexture`` 仅在最终创建纹理本身时分配托管内存，因此消除了与脚本中执行字节到纹理转换相关的垃圾收集开销。


###示例

以下示例从互联网下载 PNG 文件，将其转换为精灵，并将其分配给[图像](script-Image.html)：

````
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Networking; 
using System.Collections;

[RequireComponent(typeof(UnityEngine.UI.Image))]
public class ImageDownloader : MonoBehaviour {
    UnityEngine.UI.Image _img;
 
    void Start () {
        _img = GetComponent<UnityEngine.UI.Image>();
        Download("http://www.mysite.com/myimage.png");
    }
 
    public void Download(string url) {
        StartCoroutine(LoadFromWeb(url));
    }
 
    IEnumerator LoadFromWeb(string url)
    {
        UnityWebRequest wr = new UnityWebRequest(url);
        DownloadHandlerTexture texDl = new DownloadHandlerTexture(true);
        wr.downloadHandler = texDl;
        yield return wr.SendWebRequest();
        if(!(wr.isNetworkError || wr.isHttpError)) {
            Texture2D t = texDl.texture;
            Sprite s = Sprite.Create(t, new Rect(0, 0, t.width, t.height),
                                     Vector2.zero, 1f);
            _img.sprite = s;
        }
    }
}
````

##DownloadHandlerAssetBundle

这种专门的下载处理程序的优势在于能够将数据流式传输到 Unity 的 AssetBundle 系统。一旦 AssetBundle 系统收到足够的数据，AssetBundle 就可以作为 `UnityEngine.AssetBundle` 对象使用。仅会创建 `UnityEngine.AssetBundle` 对象的一个副本。因此大大减少了运行时内存分配以及加载 AssetBundle 带来的内存影响。此下载处理程序还允许在未完全下载的情况下部分使用 AssetBundle，因此可以流式传输资源。

所有下载和解压缩都在工作线程上进行。

AssetBundle 通过 ``DownloadHandlerAssetBundle`` 对象进行下载，该对象具有特殊的 ``assetBundle`` 属性可用于检索 AssetBundle。

由于 AssetBundle 系统的工作方式，所有 AssetBundle 都必须具有关联的地址。通常，该地址是它们所在的名义 URL（表示任何重定向之前的 URL）。在几乎任何情况下，都应该传入与传递给 UnityWebRequest 相同的 URL。使用高级 API (HLAPI) 时，此操作将自动完成。

###示例

````
using UnityEngine;
using UnityEngine.Networking; 
using System.Collections;
 
public class MyBehaviour : MonoBehaviour {
    void Start() {
        StartCoroutine(GetAssetBundle());
    }
 
    IEnumerator GetAssetBundle() {
        UnityWebRequest www = new UnityWebRequest("http://www.my-server.com");
        DownloadHandlerAssetBundle handler = new DownloadHandlerAssetBundle(www.url, uint.MaxValue);
        www.downloadHandler = handler;
        yield return www.SendWebRequest();
 
        if(www.isNetworkError || www.isHttpError) {
            Debug.Log(www.error);
        }
        else {
            // 提取 AssetBundle
            AssetBundle bundle = handler.assetBundle;
        }
    }
}
````

##DownloadHandlerAudioClip

此下载处理程序经过优化，用于下载音频文件。与使用 ``DownloadHandlerBuffer`` 来下载原始字节再从中创建 ``AudioClip`` 相比，此下载处理程序可以更方便地执行此操作。

##示例
````
using System.Collections;
using UnityEngine;
using UnityEngine.Networking;

public class AudioDownloader : MonoBehaviour {

	void Start () {
		StartCoroutine(GetAudioClip());
	}

    IEnumerator GetAudioClip() {
        using (var uwr = UnityWebRequestMultimedia.GetAudioClip("http://myserver.com/mysound.ogg", AudioType.OGGVORBIS)) {
            yield return uwr.SendWebRequest();
            if (uwr.isNetworkError || uwr.isHttpError) {
                Debug.LogError(uwr.error);
                yield break;
            }

            AudioClip clip = DownloadHandlerAudioClip.GetContent(uwr);
            // 使用音频剪辑
        }
    }	
}
````

##DownloadHandlerMovieTexture

此下载处理程序经过优化，用于下载视频文件。与使用 ``DownloadHandlerBuffer`` 来下载原始字节再从中创建 ``MovieTexture`` 相比，此下载处理程序可以更方便地执行此操作。

##示例
````
using System.Collections;
using UnityEngine;
using UnityEngine.Networking;

public class MovieDownloader : MonoBehaviour {

    void Start () {
        StartCoroutine(GetAudioClip());
    }

    IEnumerator GetAudioClip() {
        using (var uwr = UnityWebRequestMultimedia.GetMovieTexture("http://myserver.com/mysound.ogg")) {
            yield return uwr.SendWebRequest();
            if (uwr.isNetworkError || uwr.isHttpError) {
                Debug.LogError(uwr.error);
                yield break;
            }

            MovieTexture movie = DownloadHandlerMovieTexture.GetContent(uwr);
            // 使用电影纹理
        }
    }   
}
````

##DownloadHandlerScript

对于需要完全控制下载数据处理的用户，Unity 提供了 ``DownloadHandlerScript`` 类。

默认情况下，此类的实例不执行任何操作。但是，如果您从 ``DownloadHandlerScript`` 派生自己的类，则可以重写某些函数并使用它们在数据从网络到达时接收回调。

**注意：**实际下载发生在工作线程上，但所有 ``DownloadHandlerScript`` 回调都在主线程上运行。应避免在这些回调期间执行计算量很大的操作。

###要重写的函数

####ReceiveContentLength()

````
protected void ReceiveContentLength(long contentLength);
````
收到 Content-Length 标头时调用此函数。请注意，如果服务器在处理 UnityWebRequest 的过程中发送一个或多个重定向响应，则此回调可能会多次发生。

####OnContentComplete()

````
protected void OnContentComplete();
````
当 UnityWebRequest 完全从服务器下载所有数据并且已将所有接收的数据转发到 ReceiveData 回调时，将调用此函数。

####ReceiveData()

````
protected bool ReceiveData(byte[] data, long dataLength);
````
数据从远程服务器到达后调用此函数，每帧调用一次。`data` 参数包含从远程服务器接收的原始字节，`dataLength` 表示数据数组中新数据的长度。

不使用预先分配的数据缓冲区的情况下，系统每次调用此回调时都会创建一个新的字节数组，而 `dataLength` 始终等于 ``data.Length``。使用预分配的数据缓冲区的情况下，将重复使用数据缓冲区，并且必须使用 ``dataLength`` 来查找更新的字节数。

此函数需要返回值 __true__ 或 __false__。如果返回 __false__，系统会立即中止 UnityWebRequest。如果返回 __true__，则处理将正常继续。

###避免垃圾收集开销
Unity 的许多高级用户都会关注如何减少因垃圾收集而导致的 CPU 峰值。对于这些用户，UnityWebRequest 系统允许预先分配托管代码字节数组，该数组用于将下载的数据传递给 DownloadHandlerScript 的 ``ReceiveData`` 回调。

使用此函数可以完全消除使用 DownloadHandlerScript 派生类捕获下载数据时的托管代码内存分配。

要让 ``DownloadHandlerScript`` 使用预先分配的托管缓冲区运行，请向 ``DownloadHandlerScript`` 的构造函数提供一个字节数组。

**注意：**字节数组的大小限制了每帧传递给 ReceiveData 回调的数据量。如果数据经过很多帧才缓慢到达，可能是因为提供的字节数组太小。

####示例

````
using UnityEngine;
using UnityEngine.Networking; 
using System.Collections;

public class LoggingDownloadHandler : DownloadHandlerScript {

    // 标准脚本化下载处理程序 - 在每个 ReceiveData 回调时分配内存
   
    public LoggingDownloadHandler(): base() {
    }

    // 预先分配的脚本化下载处理程序
    // 重用提供的字节数组来传递数据。
    //消除内存分配。
    
    public LoggingDownloadHandler(byte[] buffer): base(buffer) {
    }

    //对于 DownloadHandler 基类而言是必需的。在处理“bytes”属性时调用。
    
    protected override byte[] GetData() { return null; }

    //从网络收到数据后每帧调用一次。
    
    protected override bool ReceiveData(byte[] data, int dataLength) {
        if(data == null || data.Length < 1) {
            Debug.Log("LoggingDownloadHandler :: ReceiveData - received a null/empty buffer");
            return false;
        }

        Debug.Log(string.Format("LoggingDownloadHandler :: ReceiveData - received {0} bytes", dataLength));
        return true;
    }

    // 从服务器收到所有数据并通过 ReceiveData 传递后调用。
    
    protected override void CompleteContent() {
        Debug.Log("LoggingDownloadHandler :: CompleteContent - DOWNLOAD COMPLETE!");
    }

    // 从服务器收到 Content-Length 标头时调用。
    
    protected override void ReceiveContentLength(int contentLength) {
        Debug.Log(string.Format("LoggingDownloadHandler :: ReceiveContentLength - length {0}", contentLength));
    }
}
````

