#发布到 iOS

Unity Cloud Build 可帮助您自动执行将 Unity 项目发布到 iOS 设备的过程。

本文介绍将项目发布到 iOS 设备所需的先决条件以及如何创建支持组件来配置 Cloud Build。涵盖的主题包括：

* 加入 Apple Developer Program
* 创建 iOS 证书和 p12 文件
* 将设备添加到配置文件
* 创建资源调配配置文件
* 配置应用程序以发布到 iOS


## 加入 Apple Developer Program

要开发 iOS 应用程序，您必须是 iOS Developer Program 的成员。目前的费用为每年 99 美元，允许您在 Apple App Store 中编译、测试并最终发布您的应用程序。

**注意**：必须具有基于 Intel 并运行 OS X Yosemite (v10.10) 或更高版本的 Mac 系统才能开发和分发 iOS 应用程序和 Mac 应用程序。

要加入 iOS Developer Program：

1.登录 [Apple Developer Program](https://developer.apple.com/programs/) 页面。

    **重要信息**：请使用 Safari 浏览器。使用 Chrome 或 Firefox 浏览器可能会遇到问题。

1.单击 **Enroll** 按钮。
1.阅读 **What You Need To Enroll** 页面上的信息，然后单击 **Start Your Enrollment** 按钮。
1.用您的 Apple ID 登录，如果没有 Apple ID，请创建一个新的 Apple ID
1.选择是注册为 Individual/Sole Proprietor/Single Person Business，还是注册为 Company/Organization
    1.**Individual/Sole Proprietor/Single Person Business**：如果您是个人/个体业主/单人公司，请选择此选项。
    1.**Company/Organization**：如果您是公司、非营利组织、合资企业、合伙企业或政府组织，请选择此选项。
1.输入您的联系信息和任何所需的业务信息。
1.阅读许可协议。如果您同意这些条款，请选中接受复选框，然后单击 **Continue** 按钮。
1.购买该计划

    **注意**：在您的成员资格被批准之前，您无法访问 Apple Developer Program。Apple 通常需要大约 24 小时来批准您的成员资格。

1.激活该计划

登录 Apple Program Developer Portal 后，您将看到左侧有一个标示 **Program Resources** 的列表。单击 **Certificates, IDs & Profiles** 即可管理您开发和分发应用程序所需的证书、标识符、配置文件和设备。

## 资源调配配置文件

资源调配配置文件会将开发者及设备与授权的开发团队联系起来，并使您能够使用设备进行测试。您必须在计划运行应用程序代码的每个设备上都安装*开发资源调配配置文件* (Development Provisioning Profile)。

每个开发资源调配配置文件均包含一组*开发证书 (Development Certificates)、唯一设备标识符 (Unique Device Identifiers, UDID)* 和一个 App ID。

要使用设备进行测试，还必须在配置文件中包括开发证书。单个设备可以包含多个资源调配配置文件。

## 资源调配配置文件的组成部分

证书将确定您的应用程序是纯开发版本还是 App Store 的候选发布版本。您应该使用临时生产证书 (Ad Hoc Production Certificate) 以便能够测试游戏的所有功能（如 GameCenter 等）。

标识符是用于标识项目的唯一 ID。对于基本项目，或者如果这是您的第一个 iOS 项目，您可能希望创建一个 App ID。此 ID 通常与您的 Unity3D 项目的捆绑 ID (Bundle ID) 相同。

**提示**：有关签名标识和证书的更多信息，请参阅 Apple 开发者网站上的[维护签名身份和证书 (Maintaining Your Signing Identities and Certificates)](https://help.apple.com/xcode/mac/current/#/dev3a05256b8)。

设备是您计划对项目进行测试的硬件平台，如 iPhone、iPad 或 iPod。您必须检索出计划进行游戏测试的每个设备的 UDID。然后，将 UDID 添加到 iOS Developer Portal 中的 Devices 部分。

**Note**: Each year, you’re allowed to register a fixed number of devices. The maximum number of devices you can register is 100 devices per product family per membership year. For more information see, [Registering Devices Using Your Developer Account](https://help.apple.com/xcode/mac/current/#/dev8b4250b57) in the **Maintaining Identifiers, Devices, and Profiles** topic on the Apple developer website.

## 创建 iOS 证书和 p12 文件

创建证书时，必须决定是创建*开发证书*（仅用于测试），还是*生产证书*（用于通过 App Store 分发应用程序）。

**提示**：创建生产证书。虽然这两种证书类型都可用于开发，但使用生产证书可以更方便地将应用程序发布到 Apple 商店。

### 创建证书

1.登录 [Apple Developer Program](https://developer.apple.com/programs/)。
1.单击 **Member Center** > **Certificates** > **Identifiers & Profiles** > **Certificates**。
1.在左栏的 **Certificates** 下，单击 **All**。
1.在标示 **What type of certificate do you need** 的屏幕中，选择要生成的证书类型。通常，如果您刚起步，需要选择 **App Store and Ad Hoc certificate production certificate**。
1.然后，在 Mac 上使用 *Keychain Access* 程序（打开 Finder 并在 *Applications/Utilities* 中查找该程序）生成证书签名请求 (CSR) 文件。按照 iOS Portal 中的说明完成此步骤。记下 CSR 文件的保存位置，因为下一步骤需要用到它。
1.从 **Generate your certificate** 屏幕中，上传 CSR 文件（可能具有扩展名 *.certSigningRequest*）。单击 **Choose File** 按钮并选择 CSR 文件，然后单击 **Generate** 按钮。
1.要将证书下载到 Mac，单击 **Your Certificate is Ready** 屏幕上的 **Download** 按钮。请确保将此文件存储在安全位置，并做好备份。

要将证书添加到密钥链，请找到证书文件并双击它。这将打开 Keychain Access 程序。如果看到弹出消息 "Do you want to add the certificate to a keychain?"，请选择登录并单击 **Add** 按钮。

### 导出 p12 文件

要使用 Unity Cloud Build 创建应用程序，必须将证书文件转换为 p12 文件。p12 文件包含私钥和证书，用于对代码进行签名。通常，如果在原生 Xcode 中开发项目，系统会在幕后为您处理此过程。

要生成 p12 文件：

1.在 Mac 上，打开 Finder，然后在 Applications/Utilities 中打开 Keychain Access 程序。
1.在左栏的密钥链下，确认已选择 **Login**。
1.在左栏的 **Category** 下，确认已选择 **My Certificates**。在 **Keychain Access** 主面板中，选择您的证书。
    
    **注意**：通常情况下，您的证书位于 **My Certificates** 下。如果在该位置未找到，请检查 **Certificates** 下是否存在。
1.从 **File menu** 中，选择 **File** > **Export Items**，或右键单击再选择 **Export**。
1.从 File Format 下拉菜单中选择 Personal Information Exchange (.p12)。
    
    **注意**：如果未选择 **Category** 下的 **Keychains and My Certificates** 下的 **Login**，则 p12 选项将灰显。
1.此时会要求您为 p12 文件创建密码。
    
    **重要信息**：请将密码记录在安全的位置。在 Unity Cloud Build 上设置 iOS 编译项目时，必须提供此密码。

## 添加设备

Apple 需要您提供打算安装应用程序的每个设备的 UDID。此要求仅仅是为了开发目的。一旦您的应用程序获准进入 App Store，它便可供任何人下载和安装；前提是他们拥有正确的 iOS 版本，并满足任何其他必要的要求。

## 查找您的 UDID

您可以使用 iTunes 检索设备的 UDID。有关检索过程的详细流程，请参阅 [WhatsMyUDID.com](http://whatsmyudid.com/)。

基本步骤为：

1.在 Mac 上打开 *iTunes*。
1.将设备（iPhone、iPad 等）连接到计算机。
1.在 *iTunes* 中，选择该设备。
1.您应该会看到一个屏幕中显示了设备的设备名称、容量和其他详细信息。要显示 UDID，请单击 **Serial #** 字段。
1.将 UDID 复制并粘贴到文档中，稍后可以从中检索它。
1.关闭 *iTunes* 并断开设备的连接。

## 在 Apple Developer Portal 中添加 UDID

您在有了设备 UDID 之后，现在可以将其添加到 Apple Developer Portal：

1.在 iOS Developer Portal 的左栏中，单击 **Devices** 下的 **All** 部分。
1.要添加新的 UDID，单击右上角的 Add 按钮 (**+**)。
1.为设备指定一个便于识别的名称，并将从 *iTunes* 获得的 UDID 复制并粘贴到 UDID 字段。
1.单击 **Continue**。

对每个设备重复以上步骤。

## 创建 App ID

您在创建 iOS 证书之后，现在可以创建一个 App ID：

1.在 Apple Developer Portal 的左栏中，单击 **App ID**。
1.在 **Register iOS App IDs** 面板的右上角，单击 Add 按钮 (**+**)。
1.在 **Registering an App ID** 面板中，输入以下信息：
    1.**App ID Description**：应用程序的名称（不含任何特殊字符）。
    1.**App ID Suffix**：如果您打算合并特定的服务（如 Game Center 或 In-App Purchases），请创建一个 Explicit ID（显式 ID）。如果不需要这些服务，则创建 **Wildcard App ID**（通用 App ID）。通用 App ID 允许将 App ID 重复用于多个项目。
    1.**App Services**：可选。指明您是否计划使用 Apple 提供的任何应用程序服务。

    有关注册 App ID 的更多信息，请参阅[维护标识符、设备和配置文件 (Maintaining Identifiers, Devices, and Profiles)](https://help.apple.com/xcode/mac/current/#/dev8b4250b57)。

1.单击 **Continue** 按钮。
1.在 **Confirm your App ID** 页面上，检查您提供的信息，然后单击 **Submit** 按钮。

## 创建资源调配配置文件

下一步是生成 *.mobileprovision* 文件。*.mobileprovision* 文件包含您的 p12 证书和 App ID 并指出了应用程序测试设备的 UDID。

1.在 Apple Developer Portal 中，单击 **Certificates, IDs & Profiles**。
1.在 Apple Developer Portal 的左栏中，选择 **Provisioning Profiles** 下的 **All**。
1.要添加新的资源调配配置文件，单击右上角的 Add 按钮 (**+**)。
1.在 **Development** 下，选择要创建的资源调配配置文件类型，然后单击 **Continue**。
    **注意**：如果您刚起步，应使用 **Distribution** > **Ad Hoc certificate**，因为此选项允许您在设备上编译并测试游戏。
1.选择要用于开发的 App ID，然后单击 **Continue** 按钮。
1.选择一个或多个开发证书，然后单击 **Continue** 按钮。
1.选择一个或多个设备，然后单击 **Continue** 按钮。
1.输入配置文件名称，然后单击 **Generate** 按钮。
1.单击 **Done** 按钮。

将生成的 *.mobileprovision* 文件下载到台式计算机。

## 配置应用程序以发布到 iOS

要配置 iOS Cloud Build，需要以下几项：

* 您的资源调配配置文件 (.mobileprovision)
* 您的 .p12 文件
* 用于保护 .p12 文件的密码

对于基本的 iOS 使用情况，此过程应该已经足够。对于包含 Xcode framework 的项目，必须执行一些额外的配置。

## 使用 Xcode Framework

要手动添加 Xcode framework，请使用 [Xcode Manipulation API](../ScriptReference/iOS.Xcode.PBXProject.html)。此 API 由 Unity iOS 团队维护并且此 API 允许您管理外部 Xcode framework。

有关采用此 API 的 Unity 项目示例，请参阅 BitBucket 上的 [UpdateXcodeProject](https://bitbucket.org/Unity-Technologies/iosnativecodesamples/src/4bf7fa2fe42037e324fd322d615c4194b0317731/NativeIntegration/Misc/UpdateXcodeProject/?at=stable) 示例项目。您可以使用该示例进行实验和学习。

该示例项目的插件之一是外部 Xcode 项目操作 DLL。此 DLL 是 Unity Bitbucket 代码仓库中提供的源代码的编译产品。添加 Xcode 项目操作功能的首选方法是将 C# 源代码文件复制到项目中的 Assets/Editor 文件夹。

可通过两种方法使用 Xcode Manipulation API：

* 使用内置的 Unity [PostProcessBuildAttribute](../ScriptReference/Callbacks.PostProcessBuildAttribute.html)（在 Unity Cloud Build 导出后方法运行之前执行它）。
* 使用 Unity Cloud Build [导出后](UnityCloudBuildPreAndPostExportMethods.html)方法（需要访问高级设置）。
