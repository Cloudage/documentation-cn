实现应用商店
====================

您的应用商店必须实现 IStore 接口；我们将在以下部分详细介绍其方法。

````
using UnityEngine.Purchasing.Extension;

public class MyStore : IStore
{
    private IStoreCallback callback;
    public void Initialize (IStoreCallback callback)
    {
        this.callback = callback;   
    }

    public void RetrieveProducts (System.Collections.ObjectModel.ReadOnlyCollection<UnityEngine.Purchasing.ProductDefinition> products)
    {
        // 获取商品信息并调用 callback.OnProductsRetrieved();
    }

    public void Purchase (UnityEngine.Purchasing.ProductDefinition product, string developerPayload)
    {
        // 启动购买流程并调用 callback.OnPurchaseSucceeded() 或 callback.OnPurchaseFailed()
    }

    public void FinishTransaction (UnityEngine.Purchasing.ProductDefinition product, string transactionId)
    {
        // 执行与交易相关的内务处理
    }
}
````
