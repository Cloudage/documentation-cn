# Unity Integrations

Unity Integrations 允许在开发工作流程中将以下 Unity 服务连接到非 Unity 工具：

* Collaborate

* Bug Reports (Alpha)

Unity Integrations 支持以下非 Unity 工具：

* Webhooks：用户定义的回调，允许 Unity 将 POST 请求发送到外部服务。

* Discord：通过 Discord 定义的 Webhook 向您团队的 Discord 通道发送通知。

* Slack：通过 Slack 定义的 Webhook 向您团队的 Slack 通道发送通知。

* JIRA（仅限 Collaborate）：对 JIRA 中的现有问题添加注释。例如，在 Collaborate 中发布更改时可随时添加问题的注释。

## 添加集成

要为工作流程添加集成，请执行以下操作：

1.登录 [Unity Services Dashboard](https://developer.cloud.unity3d.com)。

2.选择要添加集成的项目。

3.在左侧导航栏中，单击 __Settings__，然后单击 __Integrations (Beta)__。

4.单击 __NEW INTEGRATION__ 按钮。

5.在 __New Integration__ 对话框中：

    1.选择要添加集成的 Unity 服务，然后单击 __NEXT__ 按钮。

    2.选择要促使集成执行操作的事件，然后单击 __NEXT__ 按钮。

    3.选择要集成的程序，然后单击 __NEXT__ 按钮以配置该集成。

如果选择 Webhook 或 JIRA 集成，则会打开 __Configure options__ 步骤。否则，将打开所选工具的配置屏幕。

### Webhook 集成

要配置 Webhook 集成，必须提供以下信息：

| 参数| 描述 |
|:---|:---|
| Display Name| 用于标识集成列表中的集成的名称。 |
| Webhook URL| 从 Unity 服务接收 Webhook POST 请求的服务器端点的 URL。 |
| Authentication Secret| 接收端应用程序的客户端密钥。 |
| Content Type| 内容的 MIME 类型。从下拉菜单中选择数据的内容类型。 |
| Disable SSL/TLS Verification| 仅在必要时才设置此选项。
验证 SSL/TLS 证书有助于确保数据安全发送。  |

### Jira 集成

在 Collaborate 中发布更改时，可使用 Unity JIRA 集成为现有问题添加注释。

要配置 JIRA 集成，必须提供以下信息：

| 参数| 描述 |
|:---|:---|
| Display Name| 用于标识集成列表中的集成的名称。 |
| JIRA Site URL| JIRA 实例的 URL。 |
| JIRA Username| 有权将更新发布到 JIRA 实例的帐户的用户 ID。 |
| JIRA REST API Token| 用于验证向 JIRA 服务器发送的集成请求的 API 令牌。有关如何创建令牌的说明，请参阅 Atlassian 的文档。 |

配置 JIRA 集成后，在 Collaborate 中进行更改时，可通过在提交消息中引用问题标识来更新 JIRA 问题。例如，通过“I fixed the crashes caused by ISS-42”向问题“ISS-42”添加发布详情。

### Discord 集成

为了配置 Discord 集成，Unity 会调用一个应用程序以使用 Discord API 将 Webhook 注册到 Discord 通道。如果您没有 Discord 服务器，请参阅 Discord 文档：[如何创建服务器？(How do I create a server?)](https://support.discordapp.com/hc/en-us/articles/204849977-How-do-I-create-a-server-)。

要完成配置，请执行以下操作：

1.登录 Discord 帐户。

2.从 __Select a server__ 下拉菜单中选择您的 Discord 服务器。

3.从 __Select a channel__ 菜单选择要将通知发动到的通道。

4.单击 __Authorize__ 按钮。

### Slack 集成

为了配置 Slack 集成，Unity 会调用一个应用程序以使用 Slack API 将 Webhook 注册到 Slack 通道。如果您没有 Slack 服务器，请参阅 Slack 文档：[创建 Slack 工作空间 (Create a Slack workspace)](https://get.slack.help/hc/en-us/articles/206845317-Create-a-Slack-workspace)。

要完成配置，请执行以下操作：

1.登录 Slack 帐户。

2.在应用程序的右上角，选择 Slack 工作空间。

3.从 __Post to__ 下拉菜单中，选择要将通知发动到的 Slack 通道。

4.单击 __Authorize__ 按钮。

## 编辑集成

要编辑现有集成，请执行以下操作：

1.登录 [Unity Services Dashboard](https://developer.cloud.unity3d.com)。

2.选择要编辑集成的项目。

3.在左侧导航栏中，单击 __Settings__，然后单击 __Integrations (Beta)__。

4.单击要修改的集成旁边的 __EDIT__。

可进行的编辑类型取决于具体集成。对于 Slack 和 Discord 集成，可以更新显示名称或删除集成。

对于 Webhook 和 JIRA 集成，可以修改创建集成时提供的任意配置参数。


---

* <span class="page-edit">2018-05-02 Page published with [editorial review](DocumentationEditorialReview.html)
</span>
