#创建 UnityWebRequest

与任何其他对象一样，WebRequest 也可以实例化。可使用两个构造函数：

* 标准的无参数构造函数将创建一个所有设置均为空或默认值的新 UnityWebRequest。此函数中不会设置目标 URL，不会设置自定义标头，并且重定向限制设置为 32。
* 第二个构造函数采用一个字符串参数。此构造函数将 UnityWebRequest 的目标 URL 分配给字符串参数的值，除此之外，与无参数的构造函数相同。

还可以设置多个其他属性，用于跟踪状态，以及检查 UnityWebRequest 的结果。

##示例

````
UnityWebRequest wr = new UnityWebRequest(); // 完全为空
UnityWebRequest wr2 = new UnityWebRequest("http://www.mysite.com"); // 设置目标 URL

// 必须提供以下两项才能让 Web 请求正常工作
wr.url = "http://www.mysite.com";
wr.method = UnityWebRequest.kHttpVerbGET;   // 可设置为任何自定义方法，提供了公共常量

wr.useHttpContinue = false;
wr.chunkedTransfer = false;
wr.redirectLimit = 0;  // 禁用重定向
wr.timeout = 60;       // 此设置不要太小，Web 请求需要一些时间
````
