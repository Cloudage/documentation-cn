#Amazon Appstore 和 Amazon Underground Store

##扩展功能

###Amazon 用户 ID

要获取其他 Amazon 服务的当前 Amazon 用户 ID，请使用 `IAmazonExtensions`：

````
public void OnInitialized
    (IStoreController controller, IExtensionProvider extensions)
{
    string amazonUserId = 
        extensions.GetExtension<IAmazonExtensions>().amazonUserId;
    // ...
}
````
###Amazon 中的沙盒测试

要使用 Amazon 的本地沙盒 (Sandbox) 测试应用程序，请使用 `IAmazonConfiguration` 扩展配置在设备的 SD 卡上生成商品目录的 JSON 描述：

````
var builder = ConfigurationBuilder.Instance(
StandardPurchasingModule.Instance());
// 定义商品。
builder.AddProduct("someConsumable", ProductType.Consumable);
// 将商品描述写入 SD 卡中的 
// 相应位置。
builder.Configure<IAmazonConfiguration>()
	.WriteSandboxJSON(builder.products);
````

使用此方法将商品描述写入 SD 卡时，请在该测试应用程序的清单中声明 Android 权限以写入外部存储：

````
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /> 
````

请在发布之前删除此额外权限（如果有此权限）。

Amazon 沙盒现已设置好可以进行本地测试了。有关更多信息，请参阅 Amazon 的 [App Tester 文档](https://developer.amazon.com/public/apis/earn/in-app-purchasing/docs-v2/installing-and-configuring-app-tester)。
