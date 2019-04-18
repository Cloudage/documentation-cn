Social API
==========


Social API 是 Unity 访问如下社交功能的关键点：

* 用户个人资料
* 好友列表
* 成就
* 统计信息/排行榜

此 API 为不同的社交后端（例如 __GameCenter__）提供了统一的接口，主要供程序员在游戏项目中使用。

Social API 主要是一个异步 API，典型的用法是进行函数调用并该函数完成时注册回调。异步函数可能具有副作用，例如在 API 中填充某些状态变量，并且回调可能包含要处理的服务器数据。

Social 类位于 UnityEngine 命名空间中，因此该类始终可用，但其他 Social API 类保存在它们自己的命名空间 (UnityEngine.SocialPlatforms) 中。此外，Social API 的实现位于子命名空间（如 SocialPlatforms.GameCenter）中。

以下是如何使用 Social API 的示例 (JavaScript)：



````
import UnityEngine.SocialPlatforms;

function Start () {
    // 验证并注册 ProcessAuthentication 回调
    // 需要进行此调用才能继续进行 Social API 中的其他调用
    Social.localUser.Authenticate (ProcessAuthentication);
}

// 当验证完成时将调用此函数
// 请注意，如果操作成功，Social.localUser 将包含来自服务器的数据。
function ProcessAuthentication (success: boolean) {
    if (success) {
        Debug.Log ("Authenticated, checking achievements");

        // 请求加载的成就，并注册回调来处理它们
        Social.LoadAchievements (ProcessLoadedAchievements);
    }
    else
        Debug.Log ("Failed to authenticate");
}

// LoadAchievement 调用完成时将调用此函数
function ProcessLoadedAchievements (achievements: IAchievement[]) {
    if (achievements.Length == 0)
        Debug.Log ("Error: no achievements found");
    else
        Debug.Log ("Got " + achievements.Length + " achievements");
    
    // 也可以按照以下方式调用函数
    Social.ReportProgress ("Achievement01", 100.0, function(result) {
        if (result)
            Debug.Log ("Successfully reported achievement progress");
        else
            Debug.Log ("Failed to report achievement");
    });
}


````

下面是使用 C# 的相同示例。



````
using UnityEngine;
using UnityEngine.SocialPlatforms;

public class SocialExample : MonoBehaviour {
	
    void Start () {
        // 验证并注册 ProcessAuthentication 回调
        // 需要进行此调用才能继续进行 Social API 中的其他调用
        Social.localUser.Authenticate (ProcessAuthentication);
    }

    // 当验证完成时将调用此函数
    // 请注意，如果操作成功，Social.localUser 将包含来自服务器的数据。
    void ProcessAuthentication (bool success) {
        if (success) {
            Debug.Log ("Authenticated, checking achievements");

            // 请求加载的成就，并注册回调来处理它们
            Social.LoadAchievements (ProcessLoadedAchievements);
        }
        else
            Debug.Log ("Failed to authenticate");
    }

    // LoadAchievement 调用完成时将调用此函数
    void ProcessLoadedAchievements (IAchievement[] achievements) {
        if (achievements.Length == 0)
            Debug.Log ("Error: no achievements found");
        else
            Debug.Log ("Got " + achievements.Length + " achievements");
	 
        // 也可以按照以下方式调用函数
        Social.ReportProgress ("Achievement01", 100.0, result => {
            if (result)
                Debug.Log ("Successfully reported achievement progress");
            else
                Debug.Log ("Failed to report achievement");
        });
    }
}


````
有关 Social API 的更多信息，请查看 [Social API 脚本参考](../ScriptReference/Social.html)
