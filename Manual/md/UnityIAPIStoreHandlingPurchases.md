处理购买
==================

用户进行购买时，将调用应用商店的 Purchase 方法。应用商店应该引导用户完成结帐流程并调用 ``IStoreCallback`` 的 ``OnPurchaseSucceeded`` 或 ``OnPurchaseFailed`` 方法。

应用商店应提供收据和唯一的交易 ID；如果应用程序尚未通过提供的交易 ID 来处理购买，Unity IAP 将调用应用程序的 ``ProcessPurchase`` 方法。

完成交易
----------------------

应用程序确认交易已处理时，或者如果已经处理交易，Unity IAP 将调用应用商店的 FinishTransaction 方法。

应用商店应在购买后使用 FinishTransaction 来执行任何内务处理 (housekeeping)，例如关闭交易或使用可消耗商品。
