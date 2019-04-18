检索商品
===================

调用应用商店的 ``RetrieveProducts`` 方法时，该方法应该会获取最新的商品元数据以及（可选）当前用户的所有权状态。

此过程完成时，应用商店应该调用在初始化时提供给该商店的 ``IStoreCallback`` 的 ``OnProductsRetrieved`` 方法，从而提供表示可购商品的 ``ProductDescription`` 集合。

如果商品已为用户所有，应用商店可能会填写 ``ProductDescription`` 的收据和交易 ID 字段；Unity IAP 将对应用程序尚未处理的任何交易调用应用程序的 ``ProcessPurchase`` 方法。

请注意，如果用户离线，应用商店应重试，直到用户重新获得连接为止，注意避免通过激进的轮询来影响游戏性能。

处理错误
---------------

如果由于无法恢复的错误（例如开发者在进行商店配置时犯错）而无法检索商品，则应该调用 ``IStoreCallback`` 的 ``OnSetupFailed`` 方法，指出原因为 ``InitializationFailureReason``。
