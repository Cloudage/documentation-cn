购买收据
=================

Unity IAP 以 JSON 哈希（其中包含以下键和值）的形式提供购买收据：

|键|值|
|:---|:---|
|__Store__|正在使用的商店的名称，例如 **GooglePlay** 或 **AppleAppStore**|
|__TransactionID__|该交易的唯一标识符，由应用商店提供|
|__Payload__|因平台而异，详情如下。|

iOS
---

有效负载 (Payload) 因设备的 iOS 版本而异。

|iOS 版本|有效负载|
|:---|:---|
|__iOS &gt;= 7__|有效负载是以 base 64 编码的[应用程序收据](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ReceiptFields.html#/apple_ref/doc/uid/TP40010573-CH106-SW1)。|
|__iOS &lt; 7__|有效负载是 [SKPaymentTransaction transactionReceipt](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKPaymentTransaction_Class/)。|

Mac App Store
-------------

有效负载是以 base 64 编码的[应用程序收据](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ReceiptFields.html#/apple_ref/doc/uid/TP40010573-CH106-SW1)。

Google Play
-----------

有效负载是具有以下键和值的 JSON 哈希：

|键|值|
|:---|:---|
|__json__|Google 提供的 JSON 编码字符串；[INAPP_PURCHASE_DATA](http://developer.android.com/google/play/billing/billing_reference.html)|
|__signature__|Google 提供的 json 参数签名；[INAPP_DATA_SIGNATURE](http://developer.android.com/google/play/billing/billing_reference.html)|

通用 Windows 平台
-------------

有效负载是[由 Microsoft 指定](https://msdn.microsoft.com/en-US/library/windows/apps/windows.applicationmodel.store.currentapp.getappreceiptasync.aspx)的 XML 字符串

---

<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
