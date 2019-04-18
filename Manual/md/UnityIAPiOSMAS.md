# iOS App Store 和 Mac App Store

## 扩展功能

### 读取应用程序收据

[应用程序收据 (App Receipt)](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) 存储在设备的本地存储器中，可以按如下方式读取：

```
var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());
string receipt = builder.Configure<IAppleConfiguration>().appReceipt;
```

### 检查是否有支付限制

应用内购 (IAP) 可能在设备设置中受到限制，可以按以下方式检查是否存在限制：

```
var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());
bool canMakePayments = builder.Configure<IAppleConfiguration>().canMakePayments;
```

### 恢复交易

在 Apple 平台上，用户必须输入密码才能检索以前的交易，因此您的应用程序必须为用户提供一个按钮来输入密码。此过程中将会针对用户已拥有的任何商品调用 `IStoreListener` 的 `ProcessPurchase` 方法。

```
/// <summary>
/// Your IStoreListener implementation of OnInitialized.
/// </summary>
public void OnInitialized(IStoreController controller, IExtensionProvider extensions)
{
	extensions.GetExtension<IAppleExtensions> ().RestoreTransactions (result => {
		if (result) {
			// This does not mean anything was restored,
			// merely that the restoration process succeeded.
		} else {
			//Restoration failed.
		}
	});
}
```

### 刷新应用程序收据

Apple 提供了一种从服务器获取新应用程序收据的机制，通常在当前没有收据缓存在本地存储中时会使用此机制：[SKReceiptRefreshRequest](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKReceiptRefreshRequest_ClassRef/index.html#/apple_ref/occ/cl/SKReceiptRefreshRequest)。

请注意，这种情况下将提示用户输入密码。

Unity IAP 按如下形式提供此方法：

```
/// <summary>
/// IStoreListener 对 OnInitialized 的实现。
/// </summary>
public void OnInitialized(IStoreController controller, IExtensionProvider extensions)
{
	extensions.GetExtension<IAppleExtensions> ().RefreshAppReceipt (receipt => {
		// 如果请求成功，则调用此处理程序。
		//收据将是最新的应用程序收据。
		Console.WriteLine(receipt);
	},
	() => {
		//如果请求失败
		//（例如网络不可用或用户输入错误的密码），
		//则会调用此处理程序。
	});
}
```

### 购买前先询问

iOS 8 引入了一项新的家长控制功能，将其称为[购买前先询问 (Ask to Buy)](https://developer.apple.com/library/ios/technotes/tn2259/_index.html#/apple_ref/doc/uid/DTS40009578-CH1-UPDATE_YOUR_APP_FOR_ASK_TO_BUY)。

属于“购买前先询问”的购买需要经过家长批准。出现此情况时，Unity IAP 会向您的应用程序发送通知，如下所示：

```
/// 在 Unity IAP 完成初始化后调用此函数。
public void OnInitialized(IStoreController controller, IExtensionProvider extensions)
{
    extensions.GetExtension<IAppleExtensions>().RegisterPurchaseDeferredListener(product => {
        Console.WriteLine(product.definition.id);
    });
}
```

#### 在 Sandbox App Store 中启用 "Ask-to-Buy" 模拟

以下示例类说明了如何访问 `IAppleExtensions` 以便在 Sandbox App Store 中启用 Ask-to-Buy 模拟：

```
using UnityEngine;
using UnityEngine.Purchasing;

public class AppleSimulateAskToBuy : MonoBehaviour {
	public void SetSimulateAskToBuy(bool shouldSimulateAskToBuy) {
		if (Application.platform == RuntimePlatform.IPhonePlayer) {
			IAppleExtensions extensions = IAPButton.IAPButtonStoreManager.Instance.ExtensionProvider.GetExtension<IAppleExtensions>();
			extensions.simulateAskToBuy = shouldSimulateAskToBuy;
		}
	}
}
```

购买被批准或拒绝时，将调用应用商店的正常 `ProcessPurchase` 或 `OnPurchaseFailed` 监听器方法。

###交易收据
有时，属于“购买前先询问”的消耗品购买不会显示在应用程序收据中。在此情况下，无法使用该收据来验证这些购买。但是，iOS 提供包含所有购买（包括购买前先询问的购买情况）的交易收据。应使用 `IAppleExtensions` 来访问给定 `Product` 的最近交易收据字符串。

**注意**：交易收据不适用于 Mac 版本。请求 Mac 版本上的交易收据时，会产生空字符串。

```
# if UNITY_PURCHASING

using System;
using UnityEngine;
using UnityEngine.Purchasing;

public class AskToBuy : MonoBehaviour, IStoreListener
{
    // Unity IAP 对象
    private IStoreController m_Controller;
    private IAppleExtensions m_AppleExtensions;

    public AskToBuy ()
    {
        var builder = ConfigurationBuilder.Instance (StandardPurchasingModule.Instance ());
        builder.AddProduct ("100_gold_coins", ProductType.Consumable, new IDs {
            { "100_gold_coins_google", GooglePlay.Name },
            { "100_gold_coins_mac", MacAppStore.Name }
        });

        UnityPurchasing.Initialize (this, builder);
    }

    /// &lt;summary>
    /// 在 Unity IAP 完成初始化时将调用此函数。
    /// &lt;/summary>
    public void OnInitialized (IStoreController controller, IExtensionProvider extensions)
    {
        m_Controller = controller;
        m_AppleExtensions = extensions.GetExtension<IAppleExtensions> ();

        // 在 Apple 平台上，我们需要处理由 Apple 的“购买前先询问”功能导致的延期购买。
        //在非 Apple 平台上，这将不起作用；永远不会调用 OnDeferred。
        m_AppleExtensions.RegisterPurchaseDeferredListener (OnDeferred);
    }

    /// &lt;summary>
    /// 在购买完成时将调用此函数。
    /// &lt;/summary>
    public PurchaseProcessingResult ProcessPurchase (PurchaseEventArgs e)
    {
        if (Application.platform == RuntimePlatform.IPhonePlayer ||
            Application.platform == RuntimePlatform.tvOS) {
            string transactionReceipt = m_AppleExtensions.GetTransactionReceiptForProduct (e.purchasedProduct);
            Console.WriteLine (transactionReceipt);
            // 将交易收据发送到服务器进行验证
        }
        return PurchaseProcessingResult.Complete;
    }

    /// &lt;summary>
    /// Unity IAP 遇到不可恢复的初始化错误时调用。
    ///
    /// 请注意，如果互联网不可用，则不会调用此项；
    /// Unity IAP 将尝试初始化，直到互联网变为可用为止。
    /// &lt;/summary>
    public void OnInitializeFailed (InitializationFailureReason error)
    {
    }

    /// &lt;summary>
    /// 购买失败时调用。
    /// &lt;/summary>
    public void OnPurchaseFailed (Product i, PurchaseFailureReason p)
    {
    }

    /// &lt;summary>
    /// 特定于 iOS。
    /// 未成年人要求购买并提交给家长批准时，
    /// 此函数将作为 Apple 的“购买前先询问”功能的一部分
    /// 接受调用。
    ///
    /// 购买被批准或拒绝时，将触发正常的购买
    /// 事件。
    /// &lt;/summary>
    /// &lt;param name="item">Item.&lt;/param>
    private void OnDeferred (Product item)
    {
        Debug.Log ("Purchase deferred: " + item.definition.id);
    }
}

# endif // UNITY_PURCHASING
```

与应用程序收据不同，您无法在本地验证交易收据。而是必须将收据字符串发送到远程服务器进行验证。如果已使用远程服务器来验证应用程序收据，请将交易收据发送至同一 Apple 端点以接收 JSON 响应。

JSON 响应示例：

```
{
    "receipt": {
        "original_purchase_date_pst": "2017-11-15 15:25:20 America/Los_Angeles",
        "purchase_date_ms": "1510788320209",
        "unique_identifier": "0ea7808637555b2c633eb07aa1cb0894c821a6f9",
        "original_transaction_id": "1000000352597239",
        "bvrs": "0",
        "transaction_id": "1000000352597239",
        "quantity": "1",
        "unique_vendor_identifier": "01B57C2E-9E91-42FF-9B0D-4983175D6694",
        "item_id": "1141751870",
        "original_purchase_date": "2017-11-15 23:25:20 Etc/GMT",
        "product_id": "100.gold.coins",
        "purchase_date": "2017-11-15 23:25:20 Etc/GMT",
        "is_trial_period": "false",
        "purchase_date_pst": "2017-11-15 15:25:20 America/Los_Angeles",
        "bid": "com.unity3d.unityiap.demo",
        "original_purchase_date_ms": "1510788320209"
    },
    "status": 0
}
```

##拦截 Apple 推荐性购买
Apple allows you to promote [in-game purchases](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/StoreKitGuide/PromotingIn-AppPurchases/PromotingIn-AppPurchases.html) through your app’s product page. Unlike conventional in-app purchases, Apple promotional purchases initiate directly from the App Store on iOS and tvOS. The App Store then launches your app to complete the transaction, or prompts the user to download the app if it isn’t installed.

`IAppleConfiguration` `SetApplePromotionalPurchaseInterceptor` 回调方法可拦截 Apple 推荐性购买。使用此回调方法可以在向 Apple 发送购买信息之前呈现家长控制门 (parental gate)，发送分析事件，或执行其他功能。此回调会用到用户尝试购买的 `Product`。您必须调用 `IAppleExtensions.ContinuePromotionalPurchases()` 才能继续进行推荐性购买。这将启动任何排队等待的支付。

如果不设置回调，推荐性购买会立即通过，并调用 `ProcessPurchase` 来呈现结果。

**注意**：在其他平台上调用这些 API 将不起作用。

```
private IAppleExtensions m_AppleExtensions;

public void Awake() {
    var module = StandardPurchasingModule.Instance();
    var builder = ConfigurationBuilder.Instance(module);
        // 在 iOS 和 tvOS 上，我们可以拦截直接来自 App Store 的
        // 推荐性购买。
        //在其他平台上，这将不起作用；永远不会调用
        // OnPromotionalPurchase。
    builder.Configure<IAppleConfiguration>().
     SetApplePromotionalPurchaseInterceptorCallback(OnPromotionalPurchase);
    Debug.Log("Setting Apple promotional purchase interceptor callback");
}

public void OnInitialized(IStoreController controller, IExtensionProvider extensions) {
    m_AppleExtensions = extensions.GetExtension<IAppleExtensions>();
    foreach (var item in controller.products.all) {
        if (item.availableToPurchase) {
            //将所有这些商品设置为在用户的 App Store 中可见
            m_AppleExtensions.SetStorePromotionVisibility(item, AppleStorePromotionVisibility.Show);
        }
    }
}

private void OnPromotionalPurchase(Product item) {
    Debug.Log("Attempted promotional purchase: " + item.definition.id);
    // 已检测到推荐性购买。
    // 通过呈现家长控制门等方式处理此事件。
    //此处，仅出于演示目的，我们将等待五秒钟
    // 再继续购买。
    StartCoroutine(ContinuePromotionalPurchases());
}

private IEnumerator ContinuePromotionalPurchases() {
    Debug.Log("Continuing promotional purchases in 5 seconds");
    yield return new WaitForSeconds(5);
    Debug.Log("Continuing promotional purchases now");
    m_AppleExtensions.ContinuePromotionalPurchases (); // 仅限 iOS 和 tvOS
}

```

## 测试

要在 Apple 商店中进行测试，必须使用 iTunes Connect 测试帐户（可以在 iTunes Connect 中创建该帐户）。

在 iOS 设备或笔记本电脑上注销 App Store，启动您的应用程序，然后在您尝试进行购买或恢复交易时，系统会提示登录。

如果收到初始化失败通知而原因为 `NoProductsAvailable`，请按照以下列表逐项检查：

- iTunes Connect 商品标识符必须与提供给 Unity IAP 的商品标识符完全匹配
- 必须在 iTunes Connect 中为您的应用程序启用 IAP
- 商品必须已获准在 iTunes Connect 中销售
- 新建的 iTunes Connect 商品可能需要几个小时才可供购买
- 您必须同意最新的 iTunes Connect 开发者协议并提供有效的银行详细信息

## Mac App Store

When building a desktop Mac build you must select `Mac App Store validation` within Unity’s build settings.

编译应用程序后，必须使用 Bundle ID 和版本字符串来更新其 info.plist 文件。右键单击 `.app` 文件，然后单击 `show package contents`，找到 `info.plist` 文件，并将 `CFBundleIdentifier` 字符串更新为应用程序的 Bundle ID。

随后，必须对应用程序进行签名、打包和安装。您需要从 OSX 终端运行以下命令：

```
codesign -f --deep -s "3rd Party Mac Developer Application: " your.app/Contents/Plugins/unitypurchasing.bundle
codesign -f --deep -s "3rd Party Mac Developer Application: " your.app
productbuild --component your.app /Applications --sign "3rd Party Mac Developer Installer: " your.pkg
```

要对捆绑包签名，可能首先需要删除 Contents.meta 文件（如果存在）：`your.app/Contents/Plugins/unitypurchasing.bundle/Contents.meta`

为了正确安装应用包，必须先删除未打包的 .app 文件，然后再运行新创建的程序包。

随后，必须从 Applications 文件夹启动应用程序。首次启动应用程序时，系统会提示输入您的 iTunes 帐户详细信息，此时应输入 iTunes Connect 测试用户帐户的登录信息。然后，您便可以在沙盒环境中进行购买测试。

---

- <span class="page-edit">2018-05-30 Page published with [editorial review](DocumentationEditorialReview.html)
</span>
- <span class="page-history">在 [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) 的版本中添加了“购买前先询问”、交易收据和拦截收据 <span class="search-words">NewIn20173</span></span>
