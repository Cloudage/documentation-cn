应用商店扩展
================

应用商店可能会提供不符合跨平台购买流程的其他功能，例如在 Apple 的应用商店刷新应用程序收据的功能。

您应该创建一个接口来定义扩展功能，而其本身将实现 ``IStoreExtension`` 接口：

````
/// <summary>
/// 特定于应用商店的功能。
/// </summary>
public interface IMyExtensions : IStoreExtension
{
    // 用于提供用户 ID 的应用商店的假设方法。
    String GetUserStoreId();
}
````
应用程序通过 ``IExtensionProvider`` 请求扩展功能。这些应用程序进行此类操作时，Unity IAP 首先尝试将有效状态的应用商店实现转换为请求的类型。

如果该转换失败，Unity IAP 将提供由应用商店模块中 ``RegisterExtension`` 的方法注册的任何实例，如果未提供实例，则为 null。

即使在不受支持的平台上运行时，模块也应该为自己定义的扩展接口提供实例，以免强制应用程序开发者使用依赖于平台的编译方式。
