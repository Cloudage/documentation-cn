应用商店模块
=============

应用商店模块扩展了 ``AbstractPurchasingModule`` 类，充当 Unity IAP 可用于获取应用商店实例及任何配置和扩展的工厂。

开发者可以向 Unity IAP 提供多个模块，使这些模块能够使用您的自定义商店实现以及默认的由 Unity 提供的商店：

````
ConfigurationBuilder.Instance (MyCustomModule.Instance(), StandardPurchasingModule.Instance ());
````

当两个或多个模块具有可供指定平台使用的实现时，将会按照向 ``ConfigurationBuilder`` 提供模块的顺序确定优先级；``MyCustomModule`` 提供的任何实现都将优先于 ``StandardPurchasingModule`` 提供的实现。

请注意，一个模块可以支持多个应用商店；``StandardPurchasingModule`` 会处理 Unity IAP 的所有默认商店实现。
