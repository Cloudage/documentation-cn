#Unity Analytics 原始数据导出 (Raw Data Export)

##概述

Unity Analytics Raw Data Export 为您提供原始事件数据的完全访问权限。因此，您可以按照自己选择的任何方式来使用数据以便实现所需目的，例如构建自定义查询或进行数据可视化。

可通过两种方法访问 Raw Data Export 功能：

* Unity Analytics Dashboard 用户界面
* 通过调用 REST API

Raw Data Export 提供 2016 年 6 月以来的数据。

##Analytics Dashboard 用户界面
Unity Analytics Dashboard 提供了一种在不编写任何代码的情况下导出和访问原始事件数据的方法。在 Unity Analytics Dashboard ([analytics.cloud.unity3d.com](https://analytics.cloud.unity3d.com/)) 中，选择项目，然后导航至 __Analytics__ > __Raw Data Export__。

Then follow these steps in the __Export__ section of that screen:

1. Specify the __Data Set__ you want to export (such as appRunning, appStart, custom, deviceInfo, or transaction).
2.指定 __Start Date（开始日期）__，或者通过从下拉框中选择 __Previous Job Id__ 来继续导出前一作业的数据。
3.指定 __End Date（结束日期）__。
4.文件格式默认为 JSON。单击 __Export as JSON__ 下的下拉菜单并从中选择一项，即可指定偏好的文件格式。

![__Raw Data Export__ 界面上的 __Export__ 部分和 __Activity__ 表](../uploads/Main/UnityAnalyticsRawDataExport_ExportAndActivity.png)



Raw Data Export 将自动创建该作业并将其显示在界面上的 __Activity__ 表中。

作业完成后，即可从 __Download__ 列中选择文件来下载数据。


##REST（表述性状态转移）API

发送给 Unity Analytics 的每个数据点都存储在 Unity 的数据存储中。Raw Data Export API 允许您在接收和存储数据时将原始事件数据下载到文件中。

###要求
每个请求都要求使用您的 Unity Project ID (UPID) 和 API 密钥来进行 HTTP 基本身份验证。

![](../uploads/Main/AnalyticsAuthentication.png) 

###限制

* 请求期（`startDate` 到 `endDate`）不超过 31 天。
* 使用限制可能会有变化。
* 根据请求生成的原始数据文件在 Unity Analytics 中将保留 14 天。（要在 14 天后访问该数据，请提出新请求。）

###用户工作流程

要导出原始数据，请调用 Create Raw Data Export API。该请求会触发异步作业来处理数据。此操作需要的时间取决于要导出的数据量。

要获取当前状态或结果，请使用 Get Raw Data Export API 进行轮询。导出完成后，可在此 API 的响应中获得结果。结果包含文件列表和相应的下载 URL。您可以遍历 URL 并下载导出的数据。

**注意：**

* 请求或响应中的所有日期和时间都采用 UTC 时区。
* API 请求或响应使用 JSON 格式（为了增强可读性，本文档中已做格式调整）。数据文件格式是可以配置的。
* 采用 .gzip 压缩文件格式来导出数据。
* 该 API 的 URL 基地址为 [https://analytics.cloud.unity3d.com](https://analytics.cloud.unity3d.com)。

###创建 Raw Data Export

Raw Data Export 特定于项目并且特定于单个数据集（事件类型）。请求期限制为不超过 31 天。

请使用以下 HTTP 方法来创建 Raw Data Export：

````
POST api/v2/projects/{UNITY_PROJECT_ID}/rawdataexports
````

在请求的有效负载中提供参数，并采用 Content-Type 为 application/json 的 JSON 格式。

|**请求参数** |**必需还是可选** |**类型** |**描述** |
|:---|:---|:---|:---|
|__startDate__ |必需（除非已指定 continueFrom） |string |导出的开始日期（含）。以 YYYY-MM-DD 格式 (ISO-8601) 表示日期。 |
|__endDate__ |必需 |String |The end data (exclusive) of the export. The date is expressed in YYYY-MM-DD format (ISO 8601). This is the date at which to close the query. When searching for the current day, use the following day’s date. |
|__format__ |必需 |String |输出数据格式：json（换行符分隔的 json）或 tsv。 |
|__dataset__ |必需 |String |One of the following event types: appStart, appRunning, deviceInfo, custom, or transaction. |
|__continueFrom__ |可选 |String |用于继续导出数据的 Raw Data Export ID。从先前的完成点继续执行导出时可使用此参数。请参阅“继续”以了解更多信息。此情况下可以在 continueFrom 中指定先前的 Raw Data Export ID，而不指定 startDate。同时指定 continueFrom 和 startDate 是错误的。 |

在命令行中使用 cURL 的请求模板：

````
curl --user {UNITY_PROJECT_ID}:{API_KEY} --request POST --header "Content-Type: application/json" --data {REQUEST_JSON}
https://analytics.cloud.unity3d.com/api/v2/projects/{UNITY_PROJECT_ID}/rawdataexports
````

示例值：

````
UNITY_PROJECT_ID = aa43ae0a-a7a7-4016-ae96-e253bb126aa8
API_KEY = 166291ff148b2878375a8e54aebb1549
REQUEST_JSON = { "startDate": "2016-05-15" , "endDate": "2016-05-16", "format": "tsv", "dataset": "appStart" }
````

一个使用示例值的实际请求：

````
curl --user aa43ae0a-a7a7-4016-ae96-e253bb126aa8:166291ff148b2878375a8e54aebb1549 --request POST --header "Content-Type: application/json" --data '{ "startDate": "2016-05-15" , "endDate": "2016-05-16", "format": "tsv", "dataset": "appStart" }' https://analytics.cloud.unity3d.com/api/v2/projects/aa43ae0a-a7a7-4016-ae96-e253bb126aa8/rawdataexports
````

响应使用 JSON 格式的通用 Raw Data Export 响应属性。

####Raw Data Export 响应属性

|**响应属性** |**类型** |**描述** |
|:---|:---|:---|
|__id__ |String |Raw Data Export ID。 |
|__upid__ |String |Unity Project ID。 |
|__createdAt__ |String |创建时间（ISO 8601 格式）。 |
|__status__ |string |导出的当前状态。可能的值：running、completed 或 failed。 |
|__duration__ |Long |导出数据所用的时间（单位：毫秒）。 |
|__request__ |Json |请求参数。 |
|__result__ |Json |结果包含的属性可详细说明导出的数据。仅在导出成功完成后，结果才可用。请参阅下文了解结果属性。 |
|__result.size__ |Long |导出数据的总大小（单位：字节）。 |
|__result.eventCount__ |Long |导出的事件总数。 |
|__result.intraDay__ |Boolean |请求包含当前日期时，它可能不会包含当天的所有数据。如果最后一天的数据不完整，该属性为 TRUE。  |
|__result.fileList__ |Json |包含导出数据的文件的列表。没有数据时，文件列表为空。 |
|__result.fileList.name__ |String |文件名。 |
|__result.fileList.url__ |String |文件的下载 URL。文件采用 gzip 压缩格式。 |
|__result.fileList.size__ |Long |文件大小（单位：字节）。 |
|__result.fileList.date__ |String |文件包含此特定日期的事件。此日期基于事件的提交时间。同一日期可能有多个文件。日期采用 ISO 8601 格式。 |


响应示例：

````
{  
   "id":"8228d1e9-31b3-4a5e-aabe-55d9c8afa052",
   "upid":"beff3f49-b9ed-41a4-91ea-677e9b85e71e",
   "createdAt":"2016-05-10T10:10:10.100+0000",
   "status":"running",
   "duration" : 0,
   "request":{  
      "startDate":"2016-05-01",
      "endDate":"2016-05-02",
      "format":"json",
      "dataset":"appRunning"
   }
} 
````

####在先前的 Raw Data Export 基础上继续创建

在运行定期的 Raw Data Export 时，必须提供 continueFrom 参数（而不是 startDate）以确保在先前的 Raw Data Export 基础上继续导出操作。通过 GET API 或 Dashboard 可以获取先前的 Raw Data Export ID。

![Unity Analytics Project Dashboard 中先前的 Raw Data Export](../uploads/Main/AnalyticsPriorRawData.png)

###获取 Raw Data Export

请使用以下 HTTP 方法获取特定的 Raw Data Export 或正在进行的导出的状态：

````
GET api/v2/projects/{UNITY_PROJECT_ID}/rawdataexports/{raw_data_export_id}
````
所有必需参数都是 URL 路径的一部分。

请求示例：

````
curl --user {UNITY_PROJECT_ID}:${API_KEY} https://analytics.cloud.unity3d.com/api/v2/projects/{UNITY_PROJECT_ID}/rawdataexports/${ID}
````

响应是采用 JSON 格式的 Raw Data Export。该响应与“创建 Raw Data Export”中的响应相同。

响应示例：

````
{  
   "id":"6601f70e-6a0b-48ed-909f-26711af82b49",
   "status":"completed",
   "createdAt":"2016-05-21T04:41:54.000+0000",
   "duration":8631714000,
   "request":{  
      "startDate":"2016-02-11T00:00:00.000+0000",
      "endDate":"2016-03-11T00:00:00.000+0000",
      "format":"tsv",
      "dataset":"custom"
   },
   "result":{  
      "size":78355,
      "eventCount":17473,
      "fileList":[  
         {  
            "name":"headers.gz",
            "url":"https://uca-export.s3.amazonaws.com/staging/devTest/custom/appid%3DUNITY_PROJECT_ID/jid%3D6601f70e-6a0b-48ed-909f-26711af82b49/headers.gz?AWSAccessKeyId=AKIAJUXGNF66F4XPWSWA&Expires=1463872651&Signature=PnzIeeI%2FNxSOlKkLVpLcfK%2FxVpU%3D",
            "size":105
         },
         {  
            "name":"part-4b0cf376-3478-4bc8-845e-f73aff5c0be4.gz",
            "url":"https://uca-export.s3.amazonaws.com/staging/devTest/custom/appid%3DUNITY_PROJECT_ID/jid%3D6601f70e-6a0b-48ed-909f-26711af82b49/part-4b0cf376-3478-4bc8-845e-f73aff5c0be4.gz?AWSAccessKeyId=AKIAJUXGNF66F4XPWSWA&Expires=1463872651&Signature=xZk3%2BzQNTQ6yjK2Mh%2FaH338ABn8%3D",
            "size":78250,
            "date":"2016-02-13T00:00:00.000+0000"
         }
      ],
      "intraDay":false
   }
} 
````
###列出所有 Raw Data Export

请使用此 HTTP 方法获取特定项目的所有 Raw Data Export 的列表：

````
GET api/v2/projects/{UNITY_PROJECT_ID}/rawdataexports
````

所有必需参数都是 URL 路径的一部分。

请求示例：

````
curl --user {UNITY_PROJECT_ID}:${API_KEY} https://analytics.cloud.unity3d.com/api/v2/projects/${UNITY_PROJECT_ID}/rawdataexports/
````

响应是采用 JSON 格式的 Raw Data Export 的列表。请参阅“Raw Data Export 响应属性”以了解每个导出列表元素的定义。

响应示例：

````
[
{  
   "id":"6601f70e-6a0b-48ed-909f-26711af82b49",
   "status":"completed",
   "createdAt":"2016-05-21T04:41:54.000+0000",
   "duration":8631714000,
   "request":{  
      "startDate":"2016-02-11T00:00:00.000+0000",
      "endDate":"2016-03-11T00:00:00.000+0000",
      "format":"tsv",
      "dataset":"custom"
   },
   "result":{  
      "size":78355,
      "eventCount":17473,
      "fileList":[  
         {  
            "name":"headers.gz",
            "url":"https://uca-export.s3.amazonaws.com/staging/devTest/custom/appid%3DUNITY_PROJECT_ID/jid%3D6601f70e-6a0b-48ed-909f-26711af82b49/headers.gz?AWSAccessKeyId=AKIAJUXGNF66F4XPWSWA&Expires=1463872651&Signature=PnzIeeI%2FNxSOlKkLVpLcfK%2FxVpU%3D",
            "size":105
         },
         {  
            "name":"part-4b0cf376-3478-4bc8-845e-f73aff5c0be4.gz",
            "url":"https://uca-export.s3.amazonaws.com/staging/devTest/custom/appid%3DUNITY_PROJECT_ID/jid%3D6601f70e-6a0b-48ed-909f-26711af82b49/part-4b0cf376-3478-4bc8-845e-f73aff5c0be4.gz?AWSAccessKeyId=AKIAJUXGNF66F4XPWSWA&Expires=1463872651&Signature=xZk3%2BzQNTQ6yjK2Mh%2FaH338ABn8%3D",
            "size":78250,
            "date":"2016-02-13T00:00:00.000+0000"
         }
      ],
      "intraDay":false
   }
},
{  
   "id":"6601f70e-6a0b-48ed-909f-26711af82b48",
   "status":"completed",
   "createdAt":"2016-05-21T04:41:54.000+0000",
   "duration":8631714000,
   "request":{  
      "startDate":"2016-02-11T00:00:00.000+0000",
      "endDate":"2016-03-11T00:00:00.000+0000",
      "format":"tsv",
      "dataset":"custom"
   },
   "result":{  
      "size":78355,
      "eventCount":17473,
      "fileList":[  
         {  
            "name":"headers.gz",
            "url":"https://uca-export.s3.amazonaws.com/staging/devTest/custom/appid%3DUNITY_PROJECT_ID/jid%3D6601f70e-6a0b-48ed-909f-26711af82b48/headers.gz?AWSAccessKeyId=AKIAJUXGNF66F4XPWSWA&Expires=1463872651&Signature=PnzIeeI%2FNxSOlKkLVpLcfK%2FxVpU%3D",
            "size":105
         },
         {  
            "name":"part-4b0cf376-3478-4bc8-845e-f73aff5c0be4.gz",
            "url":"https://uca-export.s3.amazonaws.com/staging/devTest/custom/appid%3DUNITY_PROJECT_ID/jid%3D6601f70e-6a0b-48ed-909f-26711af82b48/part-4b0cf376-3478-4bc8-845e-f73aff5c0be4.gz?AWSAccessKeyId=AKIAJUXGNF66F4XPWSWA&Expires=1463872651&Signature=xZk3%2BzQNTQ6yjK2Mh%2FaH338ABn8%3D",
            "size":78250,
            "date":"2016-02-13T00:00:00.000+0000"
         }
      ],
      "intraDay":false
   }
}
]
````

###TSV 格式

如果选择以 TSV 格式导出，则会在 __headers.gz__ 的单独文件中提供头部。数据文件不包含头部。

示例头部文件：

````
ts	appid	type	userid	sessionid	remote_ip	platform	sdk_ver	debug_device	user_agent	submit_time	name	custom_params
````

###数据集

六种数据类型（事件类型）各不相同。请参阅下文了解模式定义。

**注意：**

* `ts` 是设备上生成事件的时间戳。请注意，由于设备时钟和接收事件的延迟，设备生成的时间戳可能会有偏差。
* `submit_time` 是 Unity Analytics 接收事件的时间戳。
* 如果记录中的某个字段没有数据，该字段的值将设置为该字段的数据类型的默认值（数字为 `0`，字符串为 `""`，布尔值为 `false`）。JSON 格式导出中的嵌套字段（例如 IAP `TransactionEvent.receipt` 字段）是此策略的一种例外情况。在采用嵌套字段的 JSON 格式导出中，任何未包含数据的字段都不会包含在该记录的 JSON 对象中。

####AppStart 事件

````
{
   "namespace":"com.unity.analytics.commons.schema",
   "name":"AppStartEvent",
   "type":"record",
   "fields":[
       {"name": "ts",   "type": "long", "default": 0}, 
       {"name": "appid", "type": "string", "default": ""},
       {"name": "type", "type": "string", "default": ""}, 
       {"name": "userid", "type": "string", "default": ""},
       {"name": "sessionid", "type": "string", "default": ""},
       {"name": "remote_ip", "type": "string", "default": ""},
       {"name": "platform", "type": "string", "default": ""},
       {"name": "sdk_ver", "type": "string", "default": ""},
       {"name": "debug_device", "type": "boolean", "default": false},
       {"name": "user_agent", "type": "string", "default": ""},
       {"name": "submit_time", "type": "long", "default": 0} 
   ]
} 
````

####AppRunning 事件

````
{
   "namespace":"com.unity.analytics.commons.schema",
   "name":"AppRunningEvent",
   "type":"record",
   "fields":[
       {"name": "ts",   "type": "long", "default": 0}, 
       {"name": "appid", "type": "string", "default": ""},
       {"name": "type", "type": "string", "default": ""},
       {"name": "userid", "type": "string", "default": ""},
       {"name": "sessionid", "type": "string", "default": ""},
       {"name": "remote_ip", "type": "string", "default": ""},
       {"name": "platform", "type": "string", "default": ""},
       {"name": "sdk_ver", "type": "string", "default": ""},
       {"name": "debug_device", "type": "boolean", "default": false},
       {"name": "user_agent", "type": "string", "default": ""},
       {"name": "submit_time", "type": "long", "default": 0}, 
       {"name": "duration", "type": "int", "default": 0}
   ]
} 
````

####自定义事件

````
{
   "namespace":"com.unity.analytics.commons.schema",
   "name":"CustomEvent",
   "type":"record",
   "fields":[
       {"name": "ts",   "type": "long", "default": 0}, 
       {"name": "appid", "type": "string", "default": ""},
       {"name": "type", "type": "string", "default": ""},
       {"name": "userid", "type": "string", "default": ""},
       {"name": "sessionid", "type": "string", "default": ""},
       {"name": "remote_ip", "type": "string", "default": ""},
       {"name": "platform", "type": "string", "default": ""},
       {"name": "sdk_ver", "type": "string", "default": ""},
       {"name": "debug_device", "type": "boolean", "default": false},
       {"name": "user_agent", "type": "string", "default": ""},
       {"name": "submit_time", "type": "long", "default": 0}, 
       {"name": "name", "type": "string", "default": ""},
       {
           "name":"custom_params",
           "type":["null",{
               "type":"map",
               "values": ["string","null"],
               "default": ""
           }],
           "default": null
       }
   ]
} 
````

####DeviceInfo 事件

````
{
   "namespace":"com.unity.analytics.commons.schema",
   "name":"DeviceInfoEvent",
   "type":"record",
   "fields":[
       {"name": "ts",   "type": "long", "default": 0}, 
       {"name": "appid", "type": "string", "default": ""},
       {"name": "type", "type": "string", "default": ""}, 
       {"name": "userid", "type": "string", "default": ""},
       {"name": "sessionid", "type": "string", "default": ""},
       {"name": "remote_ip", "type": "string", "default": ""},
       {"name": "platform", "type": "string", "default": ""},
       {"name": "sdk_ver", "type": "string", "default": ""},
       {"name": "debug_device", "type": "boolean", "default": false},
       {"name": "user_agent", "type": "string", "default": ""},
       {"name": "submit_time", "type": "long", "default": 0}, 
       {"name": "debug_build", "type": "boolean", "default": false},
       {"name": "rooted_jailbroken", "type": "boolean", "default": false},
       {"name": "processor_type", "type": "string", "default": ""},
       {"name": "system_memory_size", "type": "string", "default": ""},
       {"name": "make", "type": "string", "default": ""},
       {"name": "app_ver", "type": "string", "default": ""},
       {"name": "license_type", "type": "string", "default": ""},
       {"name": "app_install_mode", "type": "string", "default": ""},
       {"name": "model", "type": "string", "default": ""},
       {"name": "engine_ver", "type": "string", "default": ""},
       {"name": "os_ver", "type": "string", "default": ""},
       {"name": "app_name", "type": "string", "default": ""},
       {"name": "timezone", "type": "string", "default": ""},
       {"name": "ads_tracking", "type": "boolean", "default": false}
   ]
}  
````

####交易事件

````
{
   "namespace":"com.unity.analytics.commons.schema",
   "name":"TransactionEvent",
   "type":"record",
   "fields":[
       {"name": "ts",   "type": "long", "default": 0},
       {"name": "appid", "type": "string", "default": ""},
       {"name": "type", "type": "string", "default": ""},
       {"name": "userid", "type": "string", "default": ""},
       {"name": "sessionid", "type": "string", "default": ""},
       {"name": "remote_ip", "type": "string", "default": ""},
       {"name": "platform", "type": "string", "default": ""},
       {"name": "sdk_ver", "type": "string", "default": ""},
       {"name": "debug_device", "type": "boolean", "default": false},
       {"name": "user_agent", "type": "string", "default": ""},
       {"name": "submit_time", "type": "long", "default": 0},        {
           "name":"receipt",
           "type":["null",{
               "type":"record",
               "name": "receiptRecord",
               "fields":[
                   {"name": "data", "type": "string", "default": ""},
                   {"name": "signature", "type": "string", "default": ""}
               ]
           }],
           "default": null
       },
       {"name": "currency", "type": "string", "default": "USD"},
       {"name": "amount", "type": "float", "default": 0},
       {"name": "transactionid", "type": "long", "default": 0},
       {"name": "productid", "type": "string", "default": ""}
   ]
} 
````

# 字典

| 数据字段| 定义 | 数据集 |
|:---|:---|:---| 
| ts| 设备上生成事件的时间戳（单位：毫秒）。请注意，由于设备时钟和接收事件的延迟，设备生成的时间戳可能会有偏差 | 所有数据集 |
| appid| 分配给 Unity Analytics Dashboard 上的每个应用程序的 ID | 所有数据集 |
| type| 查询的事件类型（即 Custom、DeviceInfo、Transaction 等） | 所有数据集 |
| userid| Unity Analytics 为 userid 生成的标识符 | 所有数据集 |
| sessionid| Unity Analytics 为会话生成的标识符。如果游戏在处于未活动状态超过 30 分钟后重新打开，则会生成新的会话 ID | 所有数据集 |
| remote_ip| 启动会话的 IP 地址 | 所有数据集 |
| platform| 启动会话的平台 | 所有数据集 |
| sdk_ver| 用于此事件的 Unity Analytics SDK 版本。如果 sdk_ver 以 "u" 开头，则表示其来自 Analytics 工具构建的引擎。否则，它来自 Unity 5.2 以下版本的 Analytics 插件  | 所有数据集 |
| debug_device| 这是一个布尔值，显示事件是否是从开发版本发送的。对于来自 Unity Editor 的事件，设置为 TRUE | 所有数据集 |
| user_agent| User-Agent request-header 字段 | 所有数据集 |
| submit_time| Unity Analytics 收到事件的时间戳（单位：毫秒） | 所有数据集 |
| duration| 此会话已运行的持续时间（秒数），由 SDK 计算得出 | AppRunning |
| name| The name of the Custom Event (e.g. “LevelComplete”) | Custom |
| custom_params| 自定义事件参数及其相应值的列表。  | Custom |
| receipt| 包括平台应用商店（例如 App Store 和 Google Play）返回的数据 | Transaction |
| currency| 采用 ISO 4217 代码格式的付款货币代码（例如：USD、EUR、CAD 等） | Transaction |
| amount| 所用货币的总金额（十进制） | Transaction |
| transactionid| Unique identifier for this transaction, set by SDK. Each transaction is given a unique ID, not to be confused with the store’s transaction ID | Transaction |
| productid| 特定于商店的应用内购 (IAP) 商品标识符（例如 com.mygame.100coins） | Transaction |
| debug_build| 这是一个布尔值，显示事件是否是从开发版本发送的。对于来自 Unity Editor 的事件，设置为 TRUE | DeviceInfo |
| rooted_jailbroken| 这是一个布尔值，对于经过 root/越狱操作的手机，设置为 TRUE，正常手机则不设置 | DeviceInfo |
| processor_type| 设备处理器类型 | DeviceInfo |
| system_memory_size| 设备系统内存 | DeviceInfo |
| make| 设备的制造商（例如："OSXEditor"） | DeviceInfo |
| app_ver| 应用程序的版本（例如："1.0"） | DeviceInfo |
| deviceid| A unique device identifier. Note that this field has been removed and is no longer available. | DeviceInfo |
| license_type| Type of license (eg. “advanced_pro”) | DeviceInfo |
| app_install_mode| 显示是通过应用商店 ("store")、adhoc ("adhoc")、开发者安装 ("dev_release")、模拟器 ("simulator") 还是企业 ("enterprise") 安装的应用程序 | DeviceInfo |
| model| 设备型号（例如："MacBookPro11,3"） | DeviceInfo |
| engine_ver| Unity 引擎版本（例如："5.5.0a3"） | DeviceInfo |
| os_ver| 操作系统版本（例如："Mac OS X 10.11.5"） | DeviceInfo |
| app_name| Bundle ID 或包名（例如："com.Company.ProductName"） | DeviceInfo |
| timezone|  ISO code (eg. “GMT-7”) | DeviceInfo |
| ads_tracking| 这是一个布尔值，表示用户是否限制了广告跟踪 | DeviceInfo |
| adsid| Advertising Id. On iOS, collected when Unity Ads is enabled. On Android, all the time. Note that this field has been removed and is no longer available. | DeviceInfo |

---

* <span class="page-edit">2018-06-28  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">2018-06-28 - Removed UserInfo event.</span>

* <span class="page-history">2018-06-04 - Removed deviceid and adsid from DeviceInfo event. Deprecated UserInfo event.</span>
 
* <span class="page-history">2017-06-21 - 在 Unity [2017.1](https://docs.unity3d.com/2017.1/Documentation/Manual/30_search.html?q=newin20171) 中已更改将空嵌套字段包含在 JSON 导出中的做法 <span class="search-words">NewIn20171</span></span>
