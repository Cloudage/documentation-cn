# Remote Settings 脚本

使用 Unity Scripting API [RemoteSettings](../ScriptReference/RemoteSettings.html) 类可以在代码中处理设置。您可以为 `RemoteSettings.Updated` 事件注册一个处理程序函数。每当启动新会话时，`RemoteSettings` 类就会调用所有已注册的处理程序。请在 Unity Analytics Dashboard 上为需要下载的 `RemoteSettings` 对象[创建键/值对](UnityAnalyticsRemoteSettingsCreating.html)。

获取设置需要[网络处理](UnityAnalyticsRemoteSettingsNetRequests.html)，所以 `RemoteSettings` 对象通过异步方式分发 `Updated` 事件。可能不会在每个平台上甚至不会在同一平台每次启动时以相同顺序调用您的处理程序函数。事实上，如果没有可用的网络连接并且找不到缓存的设置，`RemoteSettings` 对象根本不会分发 `Updated` 事件。请始终用合理的默认值来初始化您的配置变量，并允许在不同时间或以不同顺序调用 `Updated` 处理程序。

**代码示例**

以下示例显示的一个类定义了可调整游戏难度的多个属性：

```
using UnityEngine;

public class RemoteTuningVariables : MonoBehaviour {
	
    public float DefaultSpawnRateFactor = 1.0f;
    public float DefaultEnemySpeedFactor = 1.0f;
    public float DefaultEnemyStrengthFactor = 1.0f;
    public static float SpawnRateFactor{ get; private set; }
    public static float EnemySpeedFactor{ get; private set; }
    public static float EnemyStrengthFactor{ get; private set; }

    void Start () {
        SpawnRateFactor = DefaultSpawnRateFactor;
        EnemySpeedFactor = DefaultEnemySpeedFactor;
        EnemyStrengthFactor = DefaultEnemyStrengthFactor;

        RemoteSettings.Updated += 
            new RemoteSettings.UpdatedEventHandler(HandleRemoteUpdate);
    }

    private void HandleRemoteUpdate(){
        SpawnRateFactor 
            = RemoteSettings.GetFloat ("SpawnRateFactor", DefaultSpawnRateFactor);
        EnemySpeedFactor
            = RemoteSettings.GetFloat ("EnemySpeedFactor", DefaultEnemySpeedFactor);
        EnemyStrengthFactor 
            = RemoteSettings.GetFloat ("EnemyStrengthFactor", DefaultEnemyStrengthFactor);
    }
}
```

注意，该类在 `RemoteSettings.GetFloat()` 方法调用中提供了默认值。如果 `RemoteSettings` 对象无法找到指定的键（例如，如果您拼错键名称），该方法仍会将默认值分配给调整变量。否则，`GetFloat()` 和 `GetInt()` 方法为数字分配零，`GetString()` 为字符串分配空字符串，而 `GetBool()` 为布尔值变量分配 false。

该类还向 `Start()` 方法中的属性分配相同的默认值。Unity 无法访问 Analytics 服务且本地先前未缓存配置时，`RemoteSettings` 对象不会分发 `Updated` 事件。在 `Start()` 方法中分配默认值可以确保这些属性始终有合理值。

---

* <span class="page-edit">2017-05-30 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">截至 2017-05-30，服务与 Unity 5.5 之后的版本兼容，但是版本兼容性可能会发生变化。</span>
 
* <span class="page-history">2017.1 中的新功能</span>
