Google Play
===========

消耗品
-----------

Unity IAP 使用第 3 版 Google 计费 API，其中包含可消耗商品和显式消耗 API 调用的概念。

您应该在 Google Publisher 控制面板 (Google Publisher Dashboard) 中将所有可消耗商品创建为“受管”(Managed) 商品。Unity IAP 会在应用程序确认购买时负责消耗这些商品。

测试
-------

在发布应用程序之前，必须在 Android 设备上以 Alpha 或 Beta 版形式测试应用内购 (IAP)。

请注意，虽然必须发布 Alpha 或 Beta APK 才能测试 IAP，但是这并不意味着应用程序必须在 Google Play 应用商店中公开显示。

为了对 IAP 进行完整的端到端测试，必须在使用测试帐户登录设备之后进行此测试。

请注意以下原则：

* 必须将已签名的 APK 发行版（以 Alpha 或 Beta 版发布）上传到 Google Play。
* 上传的 APK 的版本号必须与测试的 APK 的版本号匹配。
* 将购买元数据输入到 Google Play 发布者控制台之后，可能需要长达 24 小时才能使用测试帐户来购买 IAP。
