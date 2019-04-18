初始化
==============

Unity IAP 使用 ``IStoreCallback``（应用商店用来与 Unity IAP 进行异步通信）来调用应用商店的 ``Initialize`` 方法。

````
void Initialize(IStoreCallback callback) {
    // 保持对回调的引用以便与 Unity IAP 通信。
    this.callback = callback;
}
````
