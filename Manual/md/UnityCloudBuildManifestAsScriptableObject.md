# ScriptableObject 格式的编译清单

`BuildManifestObject` 是一个 [ScriptableObject](../ScriptReference/ScriptableObject.html)，可用于通过脚本访问[编译清单](UnityCloudBuildManifest.html)中的值，而无需手动加载 __UnityCloudBuildManifest.json__ TextAsset。

这是由 Unity Cloud Build 调用的导出前方法的可选参数（如果尚未写入 __UnityCloudBuildManifest.json__ TextAsset）。（请参阅有关 [JSON 格式的清单](UnityCloudBuildManifestAsJSON.html)的文档。）

以下 C# 代码示例中展示的导出前方法将根据清单中提供的 `buildNumber` 更新 `PlayerSettings` 中的 `bundleVersion`。有关导出前方法的更多信息，请参阅有关[导出前和导出后方法](UnityCloudBuildPreAndPostExportMethods.html)的文档。

````
using UnityEngine;
using UnityEditor;
using System;

public class CloudBuildHelper : MonoBehaviour
{
    public static void PreExport(UnityEngine.CloudBuild.BuildManifestObject manifest)
    {
        PlayerSettings.bundleVersion = String.Format("1.0.{0}", manifest.GetValue("buildNumber", "unknown"));
    }
}
````

这是 `BuildManifestObject` 类的公有接口：

````
namespace UnityEngine.CloudBuild
{
    public class BuildManifestObject : ScriptableObject
    {
        // 尝试获取清单值 - 如果找到键并且可以转换为类型 T，则返回 true，否则返回 false。
        public bool TryGetValue<T>(string key, out T result);
        //检索清单值，如果找不到给定的键，则抛出异常。
        public T GetValue<T>(string key);
        //设置给定键的值。
        public void SetValue(string key, object value);
        //从字典复制值。在保存字典值之前将对字典值调用 ToString()。
        public void SetValues(Dictionary<string, object> sourceDict);
        //删除所有键/值对。
        public void ClearValues();
        //返回代表当前 BuildManifestObject 的字典。
        public Dictionary<string, object> ToDictionary();
        //返回代表当前 BuildManifestObject 的 JSON 格式字符串
        public string ToJson();
        // 返回代表当前 BuildManifestObject 的 INI 格式字符串
        public override string ToString();
    }
}
````
