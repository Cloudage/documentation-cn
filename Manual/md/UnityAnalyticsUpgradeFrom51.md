将 Unity Analytics 5.1 升级到 5.2 以上版本
==================================

重新集成 Unity Analytics
---------------------------------

遵循[设置 Analytics](UnityAnalyticsOverview.html) 中的说明，其中包含以下步骤：

1.确认项目 ID 匹配
2.启用 Analytics
3.在游戏中验证

通过在“在游戏中验证”步骤中检查 SDK 版本并确认 Validator 中显示了正确的 Editor 版本（即：u5.2 或 u5.3），即可确定成功完成了升级。

![](../uploads/Main/AnalyticsValidate52.png) 

更新高级集成 (Advanced Integration) 事件
------------------------------------
如果以前执行过任何现有的高级集成 (Advanced Integration)，还需要更新命名空间和调用才能使用 5.2 以上版本的语法。遵循 [5.2 以上版本的高级集成 (Advanced Integration) 说明](UnityAnalyticsAdvancedSDK.html)进行更新。
