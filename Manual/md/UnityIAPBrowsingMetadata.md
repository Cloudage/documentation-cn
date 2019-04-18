浏览商品元数据
=========================

Unity IAP 会在初始化过程中检索本地化的商品元数据；您可以通过 ``IStoreController`` 的 Products 字段来访问这些元数据。

````
foreach (var product in controller.products.all) {
    Debug.Log (product.metadata.localizedTitle);
    Debug.Log (product.metadata.localizedDescription);
    Debug.Log (product.metadata.localizedPriceString);
}
````
