# 自定义生成函数

您可以使用生成处理程序函数来自定义在客户端上基于预制件创建生成的游戏对象时的默认行为。生成处理程序函数确保您可以完全控制游戏对象的生成方式以及销毁方式。

使用 [ClientScene.RegisterSpawnHandler](../ScriptReference/Networking.ClientScene.RegisterSpawnHandler.html) 可注册用于生成和销毁客户端游戏对象的函数。服务器直接创建游戏对象，然后通过此功能在客户端上生成这些游戏对象。此函数获取游戏对象的资源 ID 和两个函数委托：一个用于负责在客户端上创建游戏对象，另一个负责在客户端上销毁游戏对象。资源 ID 可以是动态资源 ID，也可以仅仅是要生成的预制件游戏对象上找到的资源 ID（如果有的话）。

生成/取消生成器需要具有此游戏对象签名。这是在[高级 API](UNetUsingHLAPI.html) 中定义的。

```

// 处理用于在客户端上生成游戏对象的请求
public delegate GameObject SpawnDelegate(Vector3 position, NetworkHash128 assetId);

// 处理用于在客户端上取消生成游戏对象的请求
public delegate void UnSpawnDelegate(GameObject spawned);
```

传递给生成函数的资源 ID 可在预制件的 [NetworkIdentity.assetId](../ScriptReference/Networking.NetworkIdentity-assetId.html) 上找到（自动填充）。动态资源 ID 的注册按如下方式处理：

```

// 生成新的唯一 assetId
NetworkHash128 creatureAssetId = NetworkHash128.Parse("e2656f");

// 注册新 assetId 的处理程序
ClientScene.RegisterSpawnHandler(creatureAssetId, SpawnCreature, UnSpawnCreature);

// 在现有预制件上获取 assetId
NetworkHash128 coinAssetId = coinPrefab.GetComponent<NetworkIdentity>().assetId;

// 为需要自定义生成的现有预制件注册处理程序
ClientScene.RegisterSpawnHandler(coinAssetId, SpawnCoin, UnSpawnCoin);

// 生成金币 - 在客户端上调用 SpawnCoin
NetworkServer.Spawn(gameObject, coinAssetId);

```

生成函数本身是使用委托签名实现的。以下是硬币生成器。SpawnCreature 看起来相同，但有不同的生成逻辑：

```

public GameObject SpawnCoin(Vector3 position, NetworkHash128 assetId)
{
    return (GameObject)Instantiate(m_CoinPrefab, position, Quaternion.identity);
}
public void UnSpawnCoin(GameObject spawned)
{
    Destroy(spawned);
} 
```

使用自定义生成函数时，有时在不销毁游戏对象的情况下取消生成游戏对象会很有用。这可以通过调用 [NetworkServer.UnSpawn](../ScriptReference/Networking.NetworkServer.UnSpawn.html) 来实现。这样会使消息发送到客户端以取消生成游戏对象，从而在客户端上调用自定义的取消生成函数。调用此函数时不会销毁游戏对象。

请注意，在主机上不会为本地客户端生成游戏对象，因为游戏对象已存在于服务器上。这也意味着不会调用生成处理程序函数。

## 使用自定义生成处理程序来设置游戏对象池

以下示例说明了如何使用自定义生成处理程序来设置一个非常简单的游戏对象池系统。生成和取消生成过程随后会将游戏对象放入或取出游戏对象池。

```

using UnityEngine;
using UnityEngine.Networking;
using System.Collections;

public class SpawnManager : MonoBehaviour
{
    public int m_ObjectPoolSize = 5;
    public GameObject m_Prefab;
    public GameObject[] m_Pool;

    public NetworkHash128 assetId { get; set; }
    
    public delegate GameObject SpawnDelegate(Vector3 position, NetworkHash128 assetId);
    public delegate void UnSpawnDelegate(GameObject spawned);

    void Start()
    {
        assetId = m_Prefab.GetComponent<NetworkIdentity> ().assetId;
        m_Pool = new GameObject[m_ObjectPoolSize];
        for (int i = 0; i < m_ObjectPoolSize; ++i)
        {
            m_Pool[i] = (GameObject)Instantiate(m_Prefab, Vector3.zero, Quaternion.identity);
            m_Pool[i].name = "PoolObject" + i;
            m_Pool[i].SetActive(false);
        }
        
        ClientScene.RegisterSpawnHandler(assetId, SpawnObject, UnSpawnObject);
    }

    public GameObject GetFromPool(Vector3 position)
    {
        foreach (var obj in m_Pool)
        {
            if (!obj.activeInHierarchy)
            {
                Debug.Log("Activating GameObject " + obj.name + " at " + position);
                obj.transform.position = position;
                obj.SetActive (true);
                return obj;
            }
        }
        Debug.LogError ("Could not grab GameObject from pool, nothing available");
        return null;
    }
    
    public GameObject SpawnObject(Vector3 position, NetworkHash128 assetId)
    {
        return GetFromPool(position);
    }
    
    public void UnSpawnObject(GameObject spawned)
    {
        Debug.Log ("Re-pooling GameObject " + spawned.name);
        spawned.SetActive (false);
    }
}
```

要使用此管理器，请创建一个新的空游戏对象并将其命名为“SpawnManager”。创建一个名为 *SpawnManager* 的新脚本，将上面的代码示例复制到其中，然后将其附加到**新的 SpawnManager 游戏对象。接下来，将需要生成多次的预制件拖动到 **P****refab **字段，并设置 **Object Pool Size**（默认值为 5）。

最后，在用于玩家移动的脚本中设置对 SpawnManager 的引用：

```

SpawnManager spawnManager;

void Start()
{
    spawnManager = GameObject.Find("SpawnManager").GetComponent<SpawnManager> ();
}
```

玩家逻辑可能包含用于移动并触发硬币的如下所示内容：

```

void Update()
{
    if (!isLocalPlayer)
        return;
    
    var x = Input.GetAxis("Horizontal")*0.1f;
    var z = Input.GetAxis("Vertical")*0.1f;
    
    transform.Translate(x, 0, z);

    if (Input.GetKeyDown(KeyCode.Space))
    {
        // 在客户端上调用命令函数，但在服务器上唤出
        CmdFire();
    }
}
```

在玩家的触发逻辑中使其使用游戏对象池：

```

[Command]
void CmdFire()
{
    // 在服务器上设置硬币
    var coin = spawnManager.GetFromPool(transform.position + transform.forward);  
    coin.GetComponent<Rigidbody>().velocity = transform.forward*4;
    
    // 在客户端上生成硬币，调用自定义生成处理程序
    NetworkServer.Spawn(coin, spawnManager.assetId);
    
    // 当硬币在服务器上被销毁时，它会在客户端上自动销毁
    StartCoroutine (Destroy (coin, 2.0f));
}

public IEnumerator Destroy(GameObject go, float timer)
{
    yield return new WaitForSeconds (timer);
    spawnManager.UnSpawnObject(go);
    NetworkServer.UnSpawn(go);
}
```

自动销毁会显示游戏对象如何返回到池中并在再次触发时重新使用。

