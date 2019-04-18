# Samsung Galaxy Apps

## 扩展功能

### 开发者模式测试

开发者模式允许您执行 IAP 测试，而不会产生商品的实际资金费用。首先，需要使用 `ISamsungAppsConfiguration` 实例来创建配置，并将其模式设置为 `SamsungAppsMode.AlwaysSucceed`：

````
var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());

// 启用 "开发者模式" (developer mode) 进行购买，不需要使用实际资金

// SamsungAppsMode 具有以下值：Production（开发者模式 "关闭"）、AlwaysSucceed、AlwaysFail

builder.Configure<ISamsungAppsConfiguration>().SetMode(SamsungAppsMode.AlwaysSucceed);
````

### 恢复交易

用户通过恢复交易可维持自己已购买的内容的访问权限（例如，他们升级新手机时，不会丢失旧手机上购买的所有商品）。Samsung Galaxy App Store 无需恢复以前的交易。但是，可以通过为用户提供一个允许恢复购买的按钮来提高应用程序内的可用性（例如，如果他们已将该应用程序安装在其他设备上，便可这样做）。

此过程中将会针对用户已拥有的任何商品调用 `IStoreListener` 的 `ProcessPurchase` 函数。以下示例说明了此类调用。这可以从 **Restore Purchases** 按钮进行调用：

````
/// <summary>

/// IStoreListener 对 OnInitialized 的实现。

/// </summary>

public void OnInitialized(IStoreController controller, IExtensionProvider extensions)

{

    // 针对用户已拥有的任何商品调用 ProcessPurchase 函数

    extensions.GetExtension<ISamsungAppsExtensions>().RestoreTransactions(result => {

        if (result) {

            // 这并不意味着已恢复任何对象，

            // 只表示恢复过程成功了。

        } else {

            //恢复操作已失败。

        }

    });

}
````

在 Samsung Galaxy 平台上，用户可能需要输入自己的 Samsung Galaxy App Store 密码才能检索先前的交易（如果尚未输入密码）。
