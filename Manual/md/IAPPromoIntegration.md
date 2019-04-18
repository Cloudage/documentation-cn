# 内购推荐 (IAP Promo) 集成
## 概述
**重要注意事项**：游戏必须在初始化 Unity Ads 之前先初始化 Unity IAP 才能正常使用内购推荐 (IAP Promo)。

本集成指南涵盖四个主要步骤：

* [在 Unity Editor 中准备项目](#Preparation)
* [实现](#Implementation)
    * [内购 (IAP)](#ImplementingIAP)
    * [Ads](#ImplementingAds)
* [配置推荐 (Promotions)](#ConfiguringPromotions)
* [测试集成结果](#Testing)

<a name="Preparation"></a> 
## 在 Unity Editor 中准备项目

### 设置 Unity 服务
要使用内购推荐 (IAP Promo)，您需要：

1.[配置项目以使用 Unity 服务](https://docs.unity3d.com/Manual/SettingUpProjectServices.html)。
2.在项目中启用 Unity IAP (SDK 1.17+) 和 Unity Ads (SDK 2.2+)。

#### 设置 Unity IAP
内购推荐 (IAP Promo) 需要受支持的 Unity IAP SDK 版本 (1.17+)。要获取最新 IAP SDK，请在 Services 窗口 (**Window** > **Services**) 中启用 __In-App Purchasing__，或者从 [Asset Store](https://assetstore.unity.com/packages/add-ons/services/billing/unity-iap-68207) 将其导入。如果是从 Services 窗口启用，务必在系统提示时选择**导入** (Import) 资源包。

![Enabling Unity IAP in the Editor’s Services window](../uploads/Main/Enable_IAP.png)

请参阅有关[设置 IAP](https://docs.unity3d.com/Manual/UnityIAPSettingUp.html) 的文档以了解其他信息。

#### 设置 Unity Ads
内购推荐 (IAP Promo) 需要受支持的 Unity Ads SDK 版本 (2.2+)。请从 [Asset Store](https://assetstore.unity.com/packages/add-ons/services/unity-ads-66123) 导入最新的 Unity Ads SDK 以获得此 SDK。这样就能为项目启用 Unity Ads。

请参阅[设置 Unity Ads](https://docs.unity3d.com/Manual/UnityAdsUnityIntegration.html) 以了解其他信息。

<a name="Implementation"></a> 
## 实现
在设置所需服务后，即可在游戏中实现它们。

<a name="ImplementingIAP"></a> 
### 实现 IAP
必须在初始化 Unity Ads 之前先初始化 IAP 才能使用__推荐 (Promotions)__。初始化有两个选项：无码或脚本。

#### 使用 Codeless IAP
[Codeless IAP](https://docs.unity3d.com/Manual/UnityIAPCodelessIAP.html) 能为您处理初始化。如果使用 Codeless IAP 初始化，必须在代码的其他位置调用 Unity Ads 初始化方法。

要使用 Codeless IAP，请填充__商品目录__ (Product Catalog)，然后创建 __IAP 监听器__ (IAP Listener) 来获取该目录：

1.在 Editor 中，选择 **Window** > **UnityIAP** > **IAP Catalog** 以打开 **IAP Catalog** 窗口。该窗口会列出您以前配置的所有商品。必须在__商品目录__中至少配置一个__商品__。有关设置__商品__的完整过程，请参阅 [Codeless IAP](https://docs.unity3d.com/Manual/UnityIAPCodelessIAP.html)。

2.在 **IAP Catalog** 窗口中，选择 **App Store Export** > **Cloud JSON** 以导出__商品目录__的本地副本。<br/>![将 IAP 商品目录导出到 JSON](../uploads/Main/IAP_Catalog_Window.png)

3.创建 __IAP 监听器__。选择 **Window** > **Unity IAP** > **Create IAP Listener**，然后将其添加到游戏的第一个场景。游戏一启动，监听器就会获取__商品目录__。这样可以避免在游戏请求__推荐 (Promotions)__ 而__商品__没有准备好（因为无码按钮尚未出现在场景中）时发生错误。

#### 使用脚本
如果通过脚本来[手动初始化 Unity IAP](https://docs.unity3d.com/Manual/UnityIAPInitialization.html)，则可从 ```OnInitialized()``` 回调方法中初始化 Unity Ads 以确保 IAP 始终先初始化。请参阅以下代码示例：

```
using System.Collections;
using System.Collections.Generic;

using UnityEngine;
using UnityEngine.Advertisements;
using UnityEngine.Events;
using UnityEngine.Purchasing;
using UnityEngine.UI;

[RequireComponent(typeof(AdsManager))]
public class UnityIAP : MonoBehaviour, IStoreListener
{
    private IStoreController controller;
    private AdsManager ads;

    private const string product_coins = "100.gold.coins";
    private const string product_hat = "top_hat";
    private const string product_elite = "elite_status";
    private const string product_bundle = "gem_super_box";
    public int coin_count = 0;
    public int gems_count = 0;
    public bool hat_owned = false;
    public bool elite_member = false;


    private void Awake()
    {
        this.ads = GetComponent<AdsManager>();
        // 其中 AdsManager 是您的 Ads 初始化脚本
    }

    private void Start()
    {
        Debug.Log("UnityIAP.Init()");

        // var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());

        StandardPurchasingModule module = StandardPurchasingModule.Instance();
        ProductCatalog catalog = ProductCatalog.LoadDefaultCatalog();
        ConfigurationBuilder builder = ConfigurationBuilder.Instance(module);
        IAPConfigurationHelper.PopulateConfigurationBuilder(ref builder, catalog);

        UnityPurchasing.Initialize(this, builder);
    }

    public void OnInitialized(IStoreController controller, IExtensionProvider extensions)
    {
        Debug.Log("UnityIAP.OnInitialized(...)");
        this.controller = controller;

        this.ads.Init();
        // 其中 Init() 是您的 Unity Ads 初始化方法
    }

    public void OnInitializeFailed(InitializationFailureReason error)
    {
        Debug.Log("UnityIAP.OnInitializeFailed(" + error + ")");

        this.ads.Init();
        // 如果 IAP 初始化失败，您可能仍然希望初始化 Ads
    }

    public void Buy(string productId)
    {
        Debug.Log("UnityIAP.BuyClicked(" + productId + ")");
        this.controller.InitiatePurchase(productId);
    }

    public void OnPurchaseFailed(Product item, PurchaseFailureReason r)
    {
        Debug.Log("UnityIAP.OnPurchaseFailed(" + item + ", " + r + ")");
    }

    public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs e)
    {
        string purchasedItem = e.purchasedProduct.definition.id;

        switch (purchasedItem)
        {
            case product_coins:
                Debug.Log("IAPLog: Congratualtions you are richer!");
                coin_count += 100;
                Debug.Log("IAPLog: Coin count: " + coin_count);
                break;

            case product_hat:
                hat_owned = true;
                Button topHatButton = GameObject.Find("buyTopHat").GetComponent<Button>();
                topHatButton.interactable = false;
                topHatButton.GetComponentInChildren<Text>().text = "That hat is dashing!";
                Debug.Log("IAPLog: Hat owned: " + hat_owned);
                break;

            case product_elite:
                elite_member = true;
                Button eliteButton = GameObject.Find("buyElite").GetComponent<Button>();
                eliteButton.interactable = false;
                eliteButton.GetComponentInChildren<Text>().text = "Welcome to Elite Status";
                Debug.Log("IAPLog: Elite member: " + elite_member);
                break;
            case product_bundle:
                gems_count += 5000;
                break;
        }

        return PurchaseProcessingResult.Complete;
    }
}
```

<a name="ImplementingAds"></a> 
### 实现 Unity Ads
在 IAP 初始化脚本的 ```OnInitialized()``` 回调方法中初始化 Unity Ads 可确保正确的初始化顺序。以下代码示例显示了要调用的 Unity Ads 初始化方法：

```
using System.Collections;
using System.Collections.Generic;

using UnityEngine;
using UnityEngine.Advertisements;
using UnityEngine.Events;
using UnityEngine.Purchasing;
using UnityEngine.UI;

public class AdsManager : MonoBehaviour
{
    #if UNITY_IOS
        private string gameId = "0000000"; // 在此处输入您的 iOS 游戏 ID
    #elif UNITY_ANDROID
        private string gameId = "9999999"; // 在此处输入您的 Android 游戏 ID
    #else
        private string gameId = "0123456"; // 防止 Editor 错误
    #endif

    public void Init()
    {
        Debug.Log("UnityAds.Init()");
        if (!Advertisement.isSupported || Advertisement.isInitialized)
        {
            Debug.Log("Could not initialize ads");
            return;
        }

        Debug.Log("Initializing Unity Ads with game ID: " + gameId);
        Advertisement.Initialize(gameId, false);
    }

    public void ShowAdUnit()
    {
        Debug.Log("Unity Ads Log: Ad shown");
        Advertisement.Show("testAdButton"); // 在此处输入广告的广告位 ID
    }

    public void ShowPromo()
    {
        Debug.Log("Unity Ads Log: Promo Shown");
        Advertisement.Show("testPromoButton"); // 在此处输入推荐 (Promo) 的广告位 ID
    }
}
```

**注意**：如果使用 Codeless IAP 进行 IAP 初始化，必须在代码的其他位置调用 Unity Ads 初始化方法。

<a name="ConfiguringPromotions"></a> 
## 在开发者控制面板 (Developer Dashboard) 中配置推荐 (Promotions)
导航至[开发者控制面板 (Developer Dashboard) 的内购推荐 (IAP Promo) 部分](https://iap.unityads.unity3d.com)可配置内购推荐 (IAP Promo) 优惠：

- 使用[广告位](https://docs.unity3d.com/Manual/IAPPromoPlacements)可控制__推荐 (Promotions)__ 在游戏中的显示时机和方式。
- 使用[商品](https://docs.unity3d.com/Manual/IAPPromoProducts)界面可导入__商品目录__并管理每个商品的创意资源。
- 定义[推荐 (Promotions)](https://docs.unity3d.com/Manual/IAPPromoPromotions) 的参数，例如运行时间、包含的__广告位__和__商品__以及目标用户。

<a name="Testing"></a> 
## 测试集成结果
通过实现以下示例代码来调用内购推荐 (IAP Promo) 内容：

```
public void ShowPromo()
{
    Advertisement.Show (placementID);
}
```

按 Editor 中的 **Play** 可检查__广告位__提出请求时是否显示测试广告。要查看真实的推荐创意资源，必须在生产模式下将游戏发布到设备。


<br/>
<br/>

-----
* <span class="page-edit">2018-03-01  Page published with [editorial review](DocumentationEditorialReview.html)
</span>
 
