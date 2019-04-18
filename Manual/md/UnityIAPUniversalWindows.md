#通用 Windows 平台

##模拟器

Unity IAP 功能支持 Microsoft 的应用内购 (IAP) 模拟器，此模拟器允许您在发布应用程序之前在设备上测试 IAP 购买流程。

配置 Unity IAP 时，可在初始化之前启用模拟器，如下所示：

````
var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());
builder.Configure<IMicrosoftConfiguration>().useMockBillingSystem = true;
````
确保在发布应用程序之前禁用模拟计费系统。

---

<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
