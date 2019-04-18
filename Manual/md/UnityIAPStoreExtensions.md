应用商店扩展
================

应用商店提供的独特功能可能不符合正常的跨平台购买流程。通过 Unity IAP 成功初始化时向您的应用程序提供的 ``IExtensionProvider`` 可以访问此扩展功能。

使用扩展时，无需使用依赖于平台的编译；每个扩展都带有一个假的无操作实现（在不提供扩展功能的平台上运行时会使用该实现）。

例如，以下代码段显示了 Apple 提供的 ``RefreshReceipt`` 机制（用于从 Apple 服务器获取刷新后的应用程序收据）。您可以在任何 Unity IAP 平台上编译它；如果要在非 Apple 平台（例如 Android）上运行，它将不起作用，因为绝对不会调用提供的 lambda。

````
/// <summary>
/// Unity IAP 准备好可以购买时被调用。
/// </summary>
public void OnInitialized (IStoreController controller, IExtensionProvider extensions)
{
    extensions.GetExtension<IAppleExtensions> ().RefreshAppReceipt (result => {
        if (result) {
            // 成功完成刷新。
        } else {
            //刷新失败。
        }
    });
}
````
