# 订阅和席位

要使用 Unity Editor 和 Services，必须创建 Unity Developer Network (UDN) 帐户。

创建帐户时，您将获得：

* 一个默认组织
    * 组织中的一份 Unity Personal 层级订阅（许可证）
        * 订阅许可证的一个 Editor 席位

如果您没有资格使用 Unity Personal 层级订阅，必须升级到 Unity Plus 或 Pro 层级。订阅 Plus 或 Pro 时，您将获得：

* 附加到帐户相关组织的一份订阅（许可证）。
    * 订阅许可证的一个 Editor 席位

购买更多订阅时，应通过组织进行购买。可在 [Unity ID](https://id.unity.com/) 控制面板上购买额外的订阅（请参阅[成员和组](OrgsManagingyourOrganization.html#membersandgroups)）。如果您是公司的员工，则可以将许可证组织到公司组织下，同时将您在 Unity 中的其他活动分开。有关更多信息，请参阅[管理您的组织](OrgsManagingyourOrganization.html)。

<a name="subseats"></a> 
## 订阅席位

订阅席位代表单个用户许可证，允许用户在 Editor 中进行项目协作。如果您的组织使用 Pro 或 Plus 订阅，则在组织中进行项目协作的所有用户必须具有同一层级或更高层级的 Editor 席位。如果用户具有较低层级的订阅，必须为其分配许可证中的席位。

**注意**：只有组织内的所有者 (Owner) 或管理员 (Manager) 才能分配席位，请参阅[组织角色](OrgsManagingyourOrganization.html#orgroles)。

要分配席位，请执行以下操作：

1.登录 [Unity ID](https://id.unity.com/) 控制面板。
2.在左侧导航栏中，单击 __Organizations__。
3.选择组织。
4.在左侧导航面板中，单击 __Subscriptions & Services__。
5.选择要用于分配席位的订阅。
6.单击 __Manage seats__ 按钮，然后选择组织成员以为其分配席位。
7.单击 __Assign Seat(s)__ 按钮。

所选成员会收到一封电子邮件，其中包含有关如何激活 Unity 的信息。

分配席位可让用户访问组织订阅级别的 Editor 功能。这意味着，用户可拥有 Personal 层级许可证，但也可使用分配的席位来访问更高层级的订阅许可证。

您可以随时在 [Unity 网站](https://unity3d.com/unity)上购买额外的订阅席位。有关激活 Unity 的信息，请参阅[在线激活](OnlineActivationGuide.html)。

<a name="outsideorg"></a> 
## 与组织以外的个人协作

如果希望与组织外部的个人协作而不让他们访问组织的敏感信息，请将该用户直接添加到具体项目。如果该参与者自己已拥有与组织的订阅层级相匹配的 Plus 或 Pro Editor 席位，则无需为其分配席位。

要将用户添加到具体项目，请执行以下操作：

1.登录 [Unity Services Dashboard](https://developer.cloud.unity3d.com)。
2.选择要添加用户的项目。
3.在左侧导航栏中，单击 __Settings__，然后单击 __Users__。
4.在 __Add a person or group__ 字段中，输入用户的电子邮件地址。

要允许用户访问项目的 [Collaborate](UnityCollaborate.html) 和 [Cloud Build](UnityCloudBuild.html) 功能，必须为其分配 [Unity Teams](https://unity3d.com/teams) 席位（此席位独立于与订阅关联的 Editor 席位）。如果指定的用户没有 Unity Teams 席位，则会默认为其分配一个这样的席位。如果不希望用户使用 Unity Teams 进行协作，请取消选中 __Also assign a Unity Teams Seat to this user__ 复选框。

想了解更多 Unity Teams 的相关信息，请参阅[使用 Unity Teams](OrgsWorkingwithUnityTeams.html)。

---
* <span class="page-edit">2018-04-25 Page published with [editorial review](DocumentationEditorialReview.html)
</span>
