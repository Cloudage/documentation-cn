# Analytics 事件参数

标准事件和自定义事件都允许将其他信息作为事件的一部分发送到 Analytics 服务。标准事件和自定义事件之间的唯一区别是大多数标准事件都具有必需或可选参数，这些参数优先于任何自定义参数。

以 `Dictionary<string, object>` 实例形式将自定义参数传输到相应的 [AnalyticsEvent](../ScriptReference/Analytics.AnalyticsEvent.html) 函数。该字典的键为参数名称，值为参数值。创建此字典时，请在游戏的单个版本以及版本与版本之间的事件数据中使每个参数的键名称和数据类型保持一致。例如，不要有时以数字形式发送关卡名称参数，而在其他时间又以字符串形式发送关卡名称参数。这样做会导致数据难以解读。

**注意：**自定义参数字典的键名称不要以前缀 "unity" 开头（这是为内部 Unity Analytics 事件保留的）。

Unity 会将发送到 Analytics 服务的值序列化。即使添加到字典的数据类型是字符串，它也会将数字字符解析为数字。换言之，将字符串 "_51_" 添加到参数字典等同于添加数字 _51_。

最多可以为每个事件传输十个参数。对于标准事件，此限制包括必需参数和需要赋值的任何可选参数（未用的可选参数不计入此限制）。此外，传递给事件的单个键名称和字符串值的长度不得超过 100 个字符，并且所有键名称和字符串值的总长度不得超过 500 个字符。

为了提高效率，可以将参数字典实例创建为类成员，并在每次分发事件时重复使用该字典。重复使用字典对象可避免每次发送事件时分配内存。因此会减少 C# 垃圾回收器需要清理的内存分配。在场景中分发事件的频度越高，该做法越有用。以下示例定义了一个分发自定义事件的类。该类将参数字典定义为实例变量，并在每次发送事件时设置参数值。

````
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Analytics;

public class MyCustomAnalyticsEvent : MonoBehaviour {
    private const string Name = "my_custom_event";

    private Dictionary<string, object> parameters 
        = new Dictionary<string, object>();

    void Start(){
        // 使用默认值定义参数
        parameters.Add("character_class", "Unknown");
        parameters.Add("health", 0);
        parameters.Add("xp", 0);
        parameters.Add("world_x", 0);
        parameters.Add("world_y", 0);
        parameters.Add("world_z", 0);
    }

    public bool Dispatch(string characterClass, 
                         int health, 
                         int experience, 
                         Vector3 location){
                         
        // 为特定事件设置参数值
        parameters["character_class"] = characterClass;
        parameters["health"] = health;
        parameters["xp"] = experience;
        parameters["world_x"] = location.x;
        parameters["world_y"] = location.y;
        parameters["world_z"] = location.z;

        // 发送事件
        AnalyticsResult result 
            = AnalyticsEvent.Custom(Name, parameters);
        if(result == AnalyticsResult.Ok){
            return true;
        } else {
            return false;
        }
    }
}
````

这一做法也可用于随标准事件一起发送的自定义参数。

---

* <span class="page-edit">2018-03-02 Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-edit">截至 2018-03-02，服务与 Unity 5.2 之后的版本兼容，但是版本兼容性可能会发生变化</span>

* <span class="page-history">5.2 中的新功能</span>

