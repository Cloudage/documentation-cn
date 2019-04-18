初始化
==============

您必须实现 ``IStoreListener`` 接口以供 Unity IAP 用于向应用程序通报与购买相关的事件。

调用 ``UnityPurchasing.Initialize`` 方法可启动初始化过程，从而提供监听器的实现和配置。

请注意，如果网络不可用，初始化不会失败；Unity IAP 将继续尝试在后台初始化。仅在 Unity IAP 遇到无法恢复的问题（例如配置错误或在设备设置中禁用 IAP）时，初始化才会失败。

因此，Unity IAP 所需的初始化时间量可能是任意的；如果用户处于飞行模式，则会是无限期的时间。您应该相应地设计您的应用商店，防止用户在初始化未成功完成时尝试购物。

````
using UnityEngine;
using UnityEngine.Purchasing;

public class MyIAPManager : IStoreListener {

    private IStoreController controller;
    private IExtensionProvider extensions;

    public MyIAPManager () {
        var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());
        builder.AddProduct("100_gold_coins", ProductType.Consumable, new IDs
        {
            {"100_gold_coins_google", GooglePlay.Name},
            {"100_gold_coins_mac", MacAppStore.Name}
        });

        UnityPurchasing.Initialize (this, builder);
    }

    /// &lt;summary>
    /// Unity IAP 准备好可以进行购买时调用。
    /// &lt;/summary>
    public void OnInitialized (IStoreController controller, IExtensionProvider extensions)
    {
        this.controller = controller;
        this.extensions = extensions;
    }

    /// &lt;summary>
    /// Unity IAP 遇到不可恢复的初始化错误时调用。
    ///
    /// 请注意，如果互联网不可用，则不会调用此项；Unity IAP
    /// 将尝试初始化，直到互联网变为可用。
    /// &lt;/summary>
    public void OnInitializeFailed (InitializationFailureReason error)
    {
    }

    /// &lt;summary>
    /// 购买完成时调用。
    ///
    /// 可能在 OnInitialized() 之后的任何时间调用。
    /// &lt;/summary>
    public PurchaseProcessingResult ProcessPurchase (PurchaseEventArgs e)
    {
        return PurchaseProcessingResult.Complete;
    }

    /// &lt;summary>
    /// 购买失败时调用。
    /// &lt;/summary>
    public void OnPurchaseFailed (Product i, PurchaseFailureReason p)
    {
    }
}
````
