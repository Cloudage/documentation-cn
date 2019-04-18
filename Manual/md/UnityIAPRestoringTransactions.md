恢复交易
======================

用户重新安装应用程序时，应该向用户授予他们已拥有的任何非消耗品或可续订的订阅商品。应用商店为每个用户的非消耗品和可续订的订阅商品保留一条可供 Unity IAP 检索的永久记录。在 Apple 平台上无法恢复非续订的订阅。如果您在 Apple 平台上使用非续订的订阅商品，应由您负责保留活动订阅的记录并在设备之间对订阅进行同步。

在支持交易恢复功能的平台上（例如 Google Play 和通用 Windows 应用程序），Unity IAP 会在重新安装后的第一次初始化期间自动恢复用户拥有的任何商品；系统将为每项拥有的商品调用 ``IStoreListener`` 的 ``ProcessPurchase`` 方法。

在 Apple 平台上，用户必须输入密码才能检索以前的交易，因此您的应用程序必须为用户提供一个按钮来输入密码。此过程中将会针对用户已拥有的任何商品调用 ``IStoreListener`` 的 ``ProcessPurchase`` 方法。

````
/// <summary>
/// IStoreListener 对 OnInitialized 的实现。
/// </summary>
public void OnInitialized(IStoreController controller, IExtensionProvider extensions)
{
	extensions.GetExtension<IAppleExtensions> ().RestoreTransactions (result => {
		if (result) {
			// 这并不意味着已恢复任何对象，
			// 只表示恢复过程成功了。
		} else {
			//恢复操作已失败。
		}
	});
}
````
