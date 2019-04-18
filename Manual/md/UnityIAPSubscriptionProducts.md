# 订阅商品支持

Unity IAP 通过 `SubscriptionManager` 类支持商品订阅信息查询。如需示例代码，请查看 Unity IAP SDK 1.19+ 中包含的 _IAPDemo.cs_ 脚本。

### SubscriptionManager 类方法
此类支持 Apple 商店和 Google Play 应用商店。对于 Google Play，此类仅支持使用 IAP SDK 1.19+ 购买的商品。

| **方法** | **描述** |
|:---|:---|
| `public SubscriptionInfo getSubscriptionInfo()` | 返回 `SubscriptionInfo` 对象（请参阅下文） |

### SubscriptionInfo 类方法
`SubscriptionInfo` 类包含商品的订阅相关信息。

| **方法** | **描述** |
|:---|:---|
| `public string getProductId()` | 返回商品的商店 ID。 |
| `public DateTime getPurchaseDate()` | 返回商品的购买日期。<br/>对于 Apple，购买日期是订阅的购买或续订日期。对于 Google，购买日期是订阅的原始购买日期。|
| `public Result isSubscribed()` | 返回 `Result` 枚举以指示当前是否已订阅此商品。<br/>Apple 商店中不可续订的商品返回 `Result.Unsupported` 值。Apple 商店中可自动续订的商品以及 Google Play 应用商店中的订阅商品返回 `Result.True` 或 `Result.False` 值。 |
| `public Result isExpired()` | 返回 Result 枚举以指示此商品是否已到期。<br/>* Apple 商店中不可续订的商品返回 `Result.Unsupported` 值。<br/>* Apple 商店中可自动续订的商品以及 Google Play 应用商店中的订阅商品返回 `Result.True` 或 `Result.False` 值。 |
| `public Result isCancelled()` | 返回 `Result` 枚举以指示是否已取消此商品。取消的订阅意味着商品目前为已订阅状态，但不会在下一个账单日期续订。<br/>Apple 商店中不可续订的商品返回 `Result.Unsupported` 值。Apple 商店中可自动续订的商品以及 Google Play 应用商店中的订阅商品返回 `Result.True` 或 `Result.False` 值。 |
| `public Result isFreeTrial()` | 返回 `Result` 枚举以指示此商品是否为免费试用商品。<br/> * 如果应用程序不支持 Android 版本 6+ 的应用内计费 API，则 Google Play 应用商店中的商品会返回 Result.Unsupported。<br/>Apple 商店中不可续订的商品返回 `Result.Unsupported` 值。Apple 商店中可自动续订的商品以及 Google Play 应用商店中的订阅商品返回 `Result.True` 或 `Result.False` 值。 |
| `public Result isAutoRenewing()` | 返回 `Result` 枚举以指示此商品是否为可自动续订的商品。<br/>Apple 商店中不可续订的商品返回 `Result.Unsupported` 值。Apple 商店中可自动续订的商品以及 Google Play 应用商店中的订阅商品返回 `Result.True` 或 `Result.False` 值。 |
| `public TimeSpan getRemainingTime()` | 返回 `TimeSpan` 以指示在下一个账单日期之前剩余的时间。<br/> 如果应用程序不支持 Android 版本 6+ 的应用内计费 API，则 Google Play 应用商店中的商品会返回 `TimeSpan.MaxValue`。|
| `public Result isIntroductoryPricePeriod()` | 返回 `Result` 枚举以指示此商品是否处于试销价格期间。<br/>Apple 商店中不可续订的商品返回 `Result.Unsupported` 值。Apple 商店中可自动续订的商品以及 Google Play 应用商店中的订阅商品返回 `Result.True` 或 `Result.False` 值。Google Play 应用商店中的商品返回 Result 结果。如果应用程序不支持 Android 版本 6+ 的应用内计费 API，则不支持。 |
| `public TimeSpan getIntroductoryPricePeriod()` | 返回 `TimeSpan` 以指示试销价格期间的剩余时间。<br/>没有试销价格期的订阅商品返回 `TimeSpan.Zero`。如果应用程序不支持 iOS 版本 11.2+、macOS 10.13.2+ 或 tvOS 11.2+，则 Apple 商店中的商品会返回 TimeSpan.Zero。|
| `public long getIntroductoryPricePeriodCycles()` | 返回可应用于此商品的试销价格期间的数量。<br/>如果应用程序不支持 iOS 版本 11.2+、macOS 10.13.2+ 或 tvOS 11.2+，则 Apple 商店中的商品会返回 0。|
| `public string getIntroductoryPrice()` | Returns a string to indicate the introductory price of the Product.<br/>Products with no introductory price return a `"not available"` value. Apple store Products with an introductory price return a value formatted as `“0.99USD”`. Google Play Products with an introductory price return a value formatted as `“$0.99”`. Products in the Apple store return `“not available”` if the application does not support iOS version 11.2+, macOS 10.13.2+, or tvOS 11.2+. |
| `public DateTime getExpireDate()` | 返回商品下次自动续订或到期（如果订阅已取消自动续订）的日期。<br/>如果应用程序不支持 Android 版本 6+ 的应用内计费 API，则 Google Play 应用商店中的商品会返回 TimeSpan.MaxValue。 |

----
* <span class="page-edit">2018-05-30 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 版中添加了订阅商品支持 <span class="search-words">NewIn20181</span></span>
