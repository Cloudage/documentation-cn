#标签

__标签 (Tag)__ 是可分配给一个或多个__游戏对象__的参考词。例如，可为玩家控制的角色定义“Player”标签，并为非玩家控制的角色定义“Enemy”标签。还可以使用“Collectable”标签定义玩家可在场景中收集的物品。

标签有助于识别游戏对象以便于编写脚本。通过使用标签，不需要使用拖放方式手动将游戏对象添加到脚本的公开属性，因此可以节省在多个游戏对象中使用相同脚本代码的时间。

标签对[碰撞体](CollidersOverview.html)控制脚本中的触发器很有用；例如，需要通过标签确定玩家是否与敌人、道具或可收集物进行交互。

通过设置 [GameObject.FindWithTag()](../ScriptReference/GameObject.FindWithTag.html) 函数，可以查找包含所需标签的游戏对象。以下示例便使用了 GameObject.FindWithTag()。该函数在具有“Respawn”标签的游戏对象位置实例化 `respawnPrefab`：

JavaScript：

````
var respawnPrefab : GameObject;
var respawn = GameObject.FindWithTag ("Respawn");
Instantiate (respawnPrefab, respawn.position, respawn.rotation);
````


C#：

````
using UnityEngine;
using System.Collections;

public class Example : MonoBehaviour {
    public GameObject respawnPrefab;
    public GameObject respawn;
    void Start() {
        if (respawn == null)
            respawn = GameObject.FindWithTag("Respawn");
        
        Instantiate(respawnPrefab, respawn.transform.position, respawn.transform.rotation) as GameObject;
    }
}
````




##创建新标签

在 __Inspector__ 中，游戏对象的名称下面会显示 __Tag__ 和 __Layer__ 下拉菜单。

![](../uploads/Main/TagDropdown.png) 

要创建新标签，请选择 __Add Tag...__。随即在 Inspector 中打开[标签和层管理器 (Tag and Layer Manager)](class-TagManager.html)。请注意，一旦命名了标签，以后就无法重命名。

层类似于标签，但用于定义 Unity 应该如何在场景中渲染游戏对象。请参阅有关[层](Layers.html)的文档以了解更多信息。

##应用标签

在 __Inspector__ 中，游戏对象的名称下面会显示 __Tag__ 和 __Layer__ 下拉菜单。要将现有标签应用于游戏对象，请打开 __Tags__ 下拉选单，然后选择要应用的标签。游戏对象现在便与此标签关联。


##提示

* 只能为游戏对象分配一个标签。

* Unity 包含一些未出现在标签管理器中的内置标签：

    * __Untagged__
    * __Respawn__
    * __Finish__
    * __EditorOnly__
    * __MainCamera__
    * __Player__
    * __GameController__

* 可以使用任何喜欢的词作为标签。甚至可以使用短语，但这种情况下可能需要增加 Inspector 的宽度才能看到标签的全名。
