注册应用商店
======================

通过提供对应用商店的实现和名称来调用 ``RegisterStore`` 方法（必须实现 IStore 接口）。

````
public override void Configure() {
    RegisterStore(“GooglePlay”, InstantiateMyStore());
}

private void InstantiateMyStore() {
    if (Application.platform == RuntimePlatform.Android) {
        return new MyAlternativeGooglePlayImplementation ();
    }
    return null;
}
````

应用商店名称必须与开发者为该商店定义商品时使用的名称匹配，使 Unity IAP 能够在查找该商店时使用正确的商品标识符。
