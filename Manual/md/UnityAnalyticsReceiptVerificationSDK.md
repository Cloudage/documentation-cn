收据验证
====================
以下是实现的详细信息：
 
###对于 iOS

####__receipt__ 参数
* 如果将此参数保留为 __null__，则该交易将显示在未验证收入 (Unverified Revenue) 中
* 如果编写原生 iOS 插件
    * 对于 iOS 6 和更低版本，应发送原始 SKPaymentTransaction 的 [transactionReceipt](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/SKPaymentTransaction_Class/#//apple_ref/occ/instp/SKPaymentTransaction/transactionReceipt)
    * 对于 iOS 7 和更高版本，应从应用程序捆绑包发送[应用程序收据的 base64 编码值](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)。
* 如果使用 Unibill 插件
    * 将 [PurchasableItem 的 "receipt" 属性](http://outlinegames.com/unibill-documentation/#toc-purchasableitem)作为收据传入。
* 如果使用 Prime31 插件
    * 将 StoreKitTransaction 的 "base64EncodedTransactionReceipt" 属性作为收据传入。

####__signature__ 参数
由于未使用此参数，因此传入 __null__。


###对于 Android
要验证 Android 变现，请在 Analytics Dashboard 的 Project Settings Form 中输入您的 Google 公有 API 密钥 (Google Public API Key)。

在 Google Play 中为应用内购 (IAP) 实施收据验证需要用到 Google 公有 API 密钥。您的 Google 公钥位于 Google Play Developer Console > All applications > Services & APIs > Your License Key For This Application 下。这是可选的，但如果是为 Android 平台开发并有 IAP，我们强烈建议使用它。

####__receipt__ 参数
* 如果将此参数保留为 null，则该交易将显示为未验证收入
* 如果编写原生 Android 插件
    * 传入订单的[购买数据](http://developer.android.com/google/play/billing/billing_integrate.html)（这是 JSON 格式的字符串，映射到响应 Intent 中的 INAPP_PURCHASE_DATA 键）。
* 如果使用 Unibill 插件
    * 将 [PurchasableItem 的 "receipt" 属性](http://outlinegames.com/unibill-documentation/#toc-google-play)解析为 JSON，并传入 "json" 属性的值。
* 如果使用 Prime31 插件
    * 传入 GooglePurchase "originalJson" 属性
  
####__signature__ 参数
* 如果将此参数保留为 __null__，则该交易将显示为未验证收入
* 如果编写原生 Android 插件
    * 传入[签名](http://developer.android.com/google/play/billing/billing_integrate.html)（映射到响应 Intent 中的 INAPP_DATA_SIGNATURE 键）。
* 如果使用 Unibill 插件
    * 将 [PurchasableItem 的 "receipt" 属性解析为 JSON](http://outlinegames.com/unibill-documentation/#toc-google-play)，并传入 signature 属性的值。
* 如果使用 Prime31 插件
    * 传入 GooglePurchase "signature" 属性。
