# JSON 格式的编译清单

您可以在运行时访问 JSON 格式的 [Unity Cloud Build 清单](UnityCloudBuildManifest.html)。此资源要求自定义解析逻辑或使用第三方 JSON 解析器。

以下 C# 代码示例展示了如何在 GitHub Gist ([gist.github.com/darktable/1411710](https://gist.github.com/darktable/1411710)) 上使用 MiniJSON 解析器来加载和解析编译清单：

````

using UnityEngine;
using System.Collections.Generic;
using MiniJSON;

public class MyGameObject: MonoBehavior
{
    void Start()
    {
        var manifest = (TextAsset) Resources.Load("UnityCloudBuildManifest.json");
        if (manifest != null)
        {
            var manifestDict = Json.Deserialize(manifest.text) as Dictionary<string,object>;
            foreach (var kvp in manifestDict)
            {
                // 务必检查 null 值！
                var value = (kvp.Value != null) ? kvp.Value.ToString() : "";
                Debug.Log(string.Format("Key: {0}, Value: {1}", kvp.Key, value));
            }
        }
    }
}
````
