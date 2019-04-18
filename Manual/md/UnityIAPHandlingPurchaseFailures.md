处理购买失败的情况
====================

由于多种原因（包括网络故障、付款失败或设备设置），购买可能会失败。您可能希望检查购买失败的原因并提示用户采取行动，但是请注意，并非所有商店都提供详尽的失败信息。

````
/// <summary>
/// 购买失败时调用。
/// </summary>
public void OnPurchaseFailed (Product i, PurchaseFailureReason p)
{
    if (p == PurchaseFailureReason.PurchasingUnavailable) {
        // 可能在设备设置中禁用了 IAP。
    }
}
````
