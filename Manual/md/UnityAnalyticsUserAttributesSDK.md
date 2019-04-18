用户属性
===============

Unity Analytics 可以跟踪用户的受众特征。这为您提供了可靠的方式来过滤数据以便查看不同的用户细分段。此信息是从注册时输入的玩家信息或第三方 SDK 中提取的。

````
// 引用 Unity Analytics SDK 包
  using UnityEngine.Cloud.Analytics;

  // 使用此调用来指定用户性别
  UnityAnalytics.SetUserGender(Gender gender);

  // 使用此调用来指定用户出生年份 
  UnityAnalytics.SetUserBirthYear(int birthYear);
````

|**_Input Parameters_**|||
|**_Name_** |**_Type_** |**_Description_** |
|:---|:---|
|__gender__ |enum |Gender of user can be Gender.Female, Gender.Male or Gender.Unknown. |
|__birthYear__ |int |Birth year of user. Must be 4-digit year format only. |


例如：

````
  Gender gender = Gender.Female;
    UnityAnalytics.SetUserGender(gender);

    int birthYear = 2014;
    UnityAnalytics.SetUserBirthYear(birthYear);
````
