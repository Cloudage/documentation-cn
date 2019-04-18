应用商店配置
===================

应用商店可能要求开发者在初始化期间提供额外的配置信息，利用这些信息可让您的模块注册一个配置实例以实现 ``IStoreConfiguration`` 接口：

````
var config = new MyConfiguration(); // 实现 IStoreConfiguration
BindConfiguration<MyConfiguration>(new MyConfiguration());
````

开发者请求您的配置类型的实例时，Unity IAP 首先尝试将您的应用商店实现转换为配置类型。仅当该转换失败时，才会使用通过 ``BindConfiguration`` 绑定的任何实例。
