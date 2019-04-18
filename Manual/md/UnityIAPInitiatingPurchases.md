发起购买
====================

用户想要购买商品时，应调用 ``IStoreController`` 的 ``InitiatePurchase`` 方法来识别用户想要购买的商品。

````
// 用户按下购买按钮以启动购买过程时
// 调用的方法示例。
public void OnPurchaseClicked(string productId) {
    controller.InitiatePurchase(productId);
}
````

您的应用程序将异步地收到结果通知（购买成功时调用 ``ProcessPurchase``，而购买失败时调用 ``OnPurchaseFailed``）。
