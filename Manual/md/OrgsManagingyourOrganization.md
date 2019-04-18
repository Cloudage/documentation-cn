# 管理您的组织

要创建或管理组织，请登录 [Unity ID](https://id.unity.com/) 控制面板，然后从左侧导航栏中选择 __Organizations__。

## 创建组织

可使用“组织”根据目标开发实体来隔离项目。根据需要，可以创建任意数量的组织。为了分开不同公司的活动或个人使用活动，请为每个实体使用单独的组织。

通过组织对活动进行分隔还有助于顺利实现人员变更。例如，如果员工从公司离职，您可以重新分配他们的席位，因为订阅与组织关联而不是与用户相关联。

要新建组织，请执行以下操作：

1.登录 [Unity ID](https://id.unity.com/) 控制面板。
2.在左侧导航栏中，单击 __Organizations__。
3.在 __Organizations__ 面板中，单击 __Add new__ 按钮。
4.在 __Organization Name__ 文本字段中，输入组织名称，然后单击 __Create__ 按钮。

以下部分将介绍 Unity ID 控制面板中与组织相关的信息和任务。

<a name="membersandgroups"></a> 
## Members & Groups

选择组织后，单击左侧导航栏中的 __Members & Groups__ 可查看或添加该组织内的成员和组，或分配用户角色。

### 添加用户

要与其他用户合作，必须授予他们访问您项目的权限。为此需要执行以下操作之一：

* 将该用户添加到您的组织。这样可允许该用户查看组织内的所有项目。如果用户需要修改有关订阅的信息，则可为其分配所有者 (Owner) 或管理员 (Manager) 角色。

或者：

* 将该用户直接添加到具体项目。这样可使用户访问单个项目，但无法访问组织和订阅信息。

将用户添加到组织或项目允许他们查看有关项目的信息。例如，他们可以查看项目分析 (Project Analytics) 和 Performance Reporting 结果。

为了让用户处理项目而不仅仅是查看信息，他们必须具有与组织的订阅层级相匹配的席位层级。如果他们没有自己的席位层级，必须给他们分配一个层级。有关更多信息，请参阅[订阅席位](#subs)。

<a name="orgroles"></a> 
### 组织角色

可为组织成员分配不同的管理角色。每个角色在组织内有不同的特定权限。

要为团队成员分配角色，请执行以下操作：

1.登录 [Unity ID](https://id.unity.com/) 控制面板。
2.单击左侧导航栏上的 __Organizations__。__Organizations__ 页面会列出与您的帐户相关的组织名称。
3.单击组织名称旁边的齿轮图标。
4.单击左侧导航栏中的 __Members & Groups__。此页面包含与组织关联的所有成员。
5.找到要更改角色的成员的名称，然后单击铅笔图标。
6.从标题为 __Organization Role__ 的下拉菜单中选择成员的角色。选择新角色后，单击 __Save__。

**注意：**组织管理员也可以分配成员角色。但是，他们只能分配用户 (User) 或访客 (Guest) 角色。

* __所有者__可在其组织的所有订阅中访问所有项目的所有设置。所有者是唯一可以访问该组织的支付方式、发票和账单数据的用户。

* __管理员__可在其组织的所有订阅中访问所有项目的大部分设置。管理员可添加用户，访问变现数据，执行所有者可以执行的几乎所有操作。但是，他们无法查看组织的账单和信用卡信息。

* __用户__可以读取和查看除变现数据之外的所有组织数据和 Services 数据。他们无法编辑组织数据和 Services 数据。

* __访客__是不属于组织且直接添加到项目的用户。访客无权访问组织数据。他们可以查看与项目相关的所有 Services 数据（变现数据除外），并可以占用订阅中的席位。

所有组织成员都可以访问该组织的所有项目。

要针对特定项目为成员分配不同的角色，请执行以下操作：

1.登录 [Unity Services Dashboard](https://developer.cloud.unity3d.com)。
2.选择相关项目。
3.在左侧导航栏中，单击 __Settings__，然后单击 __Users__。
4.单击 __Add a person or group__ 字段。
5.从显示的列表中选择组织用户，或输入尚未添加到组织的用户的电子邮件地址。

如果指定的用户没有 Unity Teams 席位，Unity 会自动为其分配一个席位。如果不希望用户使用 Unity Teams 进行项目协作，请取消选中 __Also assign a Unity Teams Seat to this user__ 复选框。想了解更多 Unity Teams 的相关信息，请参阅[使用 Unity Teams](OrgsWorkingwithUnityTeams.html)。

**注意**：由于 Services 是基于每个项目启用的，而每个项目都与组织绑定，因此只有该组织的所有者可以为组织关联的项目启用和禁用 Services。无论订阅层级或席位如何，都需要组织内的某些角色或权限才能使用 Services 功能或查看 Unity Services Dashboard 上的相关数据。

<a name="subs"></a> 
## Subscriptions & Services

选择组织后，单击左侧导航栏中的 __Subscriptions & Services__ 可管理 Editor 和 Unity Teams 订阅。

要查看订阅的状态，请从列表中选择要查看的订阅。在订阅详细信息页面中可添加和分配该订阅的席位。此外还可以延期或升级订阅。

## Service Usage

选择组织后，单击左侧导航栏中的 __Service Usage__ 可查看 Unity Teams 云存储的分配和使用情况。

Unity Teams 包含一些云存储，您可以按月调整。单击 __Manage storage__ 可增加或减少空间。有关管理存储的更多信息，请参阅知识库中的[如何获得更多云存储 (How Do I Get More Cloud Storage)](https://support.unity3d.com/hc/en-us/articles/115005492443-How-do-I-get-more-cloud-storage-)。

## Edit Organization

选择组织后，单击左侧导航栏中的 __Edit Organization__ 可更改组织的名称和联系信息。只有所有者才能更改这些设置。

## Payment Methods

选择组织后，单击左侧导航栏中的 __Payment Methods__ 可管理 Unity 为组织保存的信用卡信息。购买订阅或云存储需要有效的付款方式。

## Transaction History 和 Payouts

选择组织后，单击左侧导航栏中的 __Transaction History__ 可查看购买和付款记录。选择要查询的日期范围，然后单击 __Apply__ 来查看该期间的记录。

__Purchases__ 选项卡会显示指定期间的购买记录。__Payouts__ 选项卡显示从 Unity 收到的广告收入付款。要接收付款，必须通过单击左侧导航栏中的 __Payment Profile__ 来设置付款资料。有关 Unity Ads 收入的更多信息，请参阅[收入和付款 (Revenue and payment)](https://unityads.unity3d.com/help/monetization/Revenue-and-payment)。

---
* <span class="page-edit">2018-04-25 Page published with [editorial review](DocumentationEditorialReview.html)
</span>
