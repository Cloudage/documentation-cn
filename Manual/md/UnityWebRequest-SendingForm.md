#将表单发送到 HTTP 服务器 (POST)
有两个主要函数可用于将数据作为 HTML 表单格式发送到服务器。如果是从 WWW 系统进行迁移，请参阅下文中的[使用 WWWForm](#UsingWWWForm)。

## 使用 IMultipartFormSection
为了在更大程度上控制表单数据的指定方式，``UnityWebRequest`` 系统包含了一个可由用户实现的 ``IMultipartFormSection`` 接口。对于标准应用程序，Unity 还提供了数据和文件部分的默认实现：``MultipartFormDataSection`` 和 ``MultipartFormFileSection``。

``UnityWebRequest.POST`` 的重载可接受成员必须全部是 ``IMultipartFormSections`` 的列表参数（作为第二个参数）。函数签名为：

````
WebRequest.Post(string url, List<IMultipartFormSection> formSections);
````

###详细信息

* 此函数将创建 `UnityWebRequest` 并将目标 URL 设置为第一个字符串参数。此函数还会为 `IMultipartFormSection` 对象列表中所指定的表单数据相应设置 `UnityWebRequest` 的 Content-Type 标头。
* 默认情况下，此函数将 `DownloadHandlerBuffer` 附加到 `UnityWebRequest`。这是为了方便起见，可用于检查服务器的应答。
* 类似于 `WWWForm POST` 函数，此 HLAPI 函数依次调用每个提供的 `IMultipartFormSection`，然后将它们的格式设置为标准多部分表单（符合 RFC 2616 中的规定）。
* 已预先设置格式的表单数据存储在标准 `UploadHandlerRaw` 对象中，此对象随后附加到 `UnityWebRequest`。因此，在 `UnityWebRequest.POST` 调用之后对 `IMultipartFormSection` 对象执行的更改不会反映在发送到服务器的数据中。

###示例

````
using UnityEngine;
using UnityEngine.Networking;
using System.Collections;
 
public class MyBehavior : MonoBehaviour {
    void Start() {
        StartCoroutine(Upload());
    }
 
    IEnumerator Upload() {
        List<IMultipartFormSection> formData = new List<IMultipartFormSection>();
        formData.Add( new MultipartFormDataSection("field1=foo&field2=bar") );
        formData.Add( new MultipartFormFileSection("my file data", "myfile.txt") );

        UnityWebRequest www = UnityWebRequest.Post("http://www.my-server.com/myform", formData);
        yield return www.SendWebRequest();
 
        if(www.isNetworkError || www.isHttpError) {
            Debug.Log(www.error);
        }
        else {
            Debug.Log("Form upload complete!");
        }
    }
}
````

<a name="UsingWWWForm"></a> 
##使用 WWWForm（旧版函数）

为了帮助从 WWW 系统进行迁移，UnityWebRequest 系统允许使用旧的 WWWForm 对象来提供表单数据。

在这种情况下，函数签名为：

````
WebRequest.Post(string url, WWWForm formData);
````

###详细信息

* 此函数创建新的 `UnityWebRequest` 并将目标 URL 设置为第一个字符串参数的值。此函数还读取由 `WWWForm` 参数生成的所有自定义标头（比如 Content-Type），然后将这些标头复制到 `UnityWebRequest` 中。
* 默认情况下，此函数将 `DownloadHandlerBuffer` 附加到 `UnityWebRequest`。这是为了方便起见，可用于检查服务器的应答。
* 此函数读取由 `WWWForm object` 生成的原始数据，然后将其缓冲在 `UploadHandlerRaw` 对象中，此对象附加到 `UnityWebRequest`。因此，调用 `UnityWebRequest.POST` 之后对 `WWWForm` 对象进行的更改不会改变 `UnityWebRequest` 的内容。

###示例

````
using UnityEngine;
using System.Collections;
 
public class MyBehavior : public MonoBehaviour {
    void Start() {
        StartCoroutine(Upload());
    }
 
    IEnumerator Upload() {
        WWWForm form = new WWWForm();
        form.AddField("myField", "myData");
 
        UnityWebRequest www = UnityWebRequest.Post("http://www.my-server.com/myform", form);
        yield return www.SendWebRequest();
 
        if(www.isNetworkError || www.isHttpError) {
            Debug.Log(www.error);
        }
        else {
            Debug.Log("Form upload complete!");
        }
    }
}
````
