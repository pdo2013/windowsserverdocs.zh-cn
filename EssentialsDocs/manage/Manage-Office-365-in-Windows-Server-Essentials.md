---
title: "管理 Windows Server Essentials 中的 Office 365"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
0author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b45bddb657b96c7cc5f9291a6c887b9d0801974b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-office-365-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 Office 365

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

当您使用 Microsoft Office 365 集成 Windows Server Essentials 服务器时，你可以以及 Windows Server Essentials 仪表板中你的本地资源管理你的 Office 365 服务和在线帐户。 在本主题中，您将了解能够通过与 Office 365 集成服务器、操作方法，以及如何管理和解决你的 Office 365 集成。  
  
  
  
> [!IMPORTANT]
>   Office 365 集成才支持在单个域控制器环境中。 此外，Office 365 集成向导必须运行域控制器上。  
  
## <a name="in-this-topic"></a>在本主题  
  
-   [为什么应该我集成 Office 365 与我的服务器？](#BKMK_IntegrationOverview)  
  
-   [设置 Office 365 集成](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [管理 Office 365 集成](#BKMK_ManageIntegration)  
  
-   [Office 365 集成疑难解答](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a>为什么应该我集成 Office 365 与我的服务器？  
 有了大量良好的原因，若要将 Office 365 集成与 Windows Server Essentials 服务器。 如果你管理一些内部资源，但其他服务使用的 Office 365，你将能够从仪表板上，你的本地资源，而不是在两个位置工作以及管理你的 Office 365 服务和资源。  
  
-   管理用户帐户以及让 Office 365 你用户访问在线帐户：  
  
    -   创建 Microsoft Online 服务的您的用户的帐户，在一个步骤中，或在服务器上的现有在线帐户创建用户帐户。 你还可以添加到新的或现有的用户帐户的在线帐户。  
  
         从仪表板中创建在线帐户时，用户登录到 Office 365 与他们在服务器上使用相同的密码。 如果他们更改用户帐户的密码，还更改的联机密码。 并且你知道自己在线帐户的密码始终满足你的用户帐户设置的安全要求。  
  
    -   管理用户帐户的用户帐户的生命周期以及在线帐户。 停用的用户帐户时，如果在线帐户被还停用。 如果你要删除的用户帐户，联机也被删除帐户。  
  
    -   在 Windows Server Essentials 服务器上，还管理 Exchange Online distribution 组的电子邮件。  
  
-   发送和接收电子邮件来自你的组织的 Internet 域 (例如，contoso.com) 通过链接到你的 Office 365 订阅的自定义 Internet 域。  
  
-   从仪表板中管理你的订阅和 Office 365 集成。  
  
-   如果你的订阅包括 SharePoint Online 库，请与 Windows Server Essentials 服务器的 Office 365 集成，您可以：  
  
    -   创建和管理仪表板中你 SharePoint Online 库。  
  
        > [!NOTE]
        >  你将也将无法使用我的 Server 2012 R2 应用能够从你的笔记本电脑、移动设备或 Windows phone SharePoint Online 库中的文档。 此功能非常仅适用于 Windows Server Essentials。 有关详细信息，请参阅[使用我的服务器应用](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。  
  
    -   从仪表板上，更改团队 SharePoint Online 站点的权限，或从若要更改其他仪表板中打开团队站点。  
  
     有关详细信息，请参阅[管理 SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md)。  
  
-   如果您订阅 Exchange Online，可以使用 Office 365 集成与 Windows Server Essentials 服务器管理您的用户使用连接到你的公司电子邮件服务器移动设备：  
  
    -   移动设备连接到公司的电子邮件服务器时，将需要密码的保护。 设置密码长度至少、的数量登录尝试失败允许，以及最低登录尝试之间所需的时间。  
  
    -   阻止移动设备连接到 Exchange Online 中，如果你知道的与设备型号的安全问题。  
  
    -   如果移动设备已丢失或被盗，擦除该设备删除设备处于打开状态下一次的任何敏感公司的数据。  
  
-    Office 365 集成为你提供了新的方法可连接到 Office 365 服务和资源：  
  
    -   从 Windows Server Essentials 启动栏中打开 Office 365 服务。 有关信息，请参阅[快速启动指南使用 Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)。  
  
    -   使用我的 Server 2012 R2 应用能够从你的笔记本电脑、移动设备或 Windows phone SharePoint Online 库中的文档。 有关信息，请参阅[使用我的服务器应用](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。 此功能将仅适用于 Windows Server Essentials。  
  
##  <a name="BKMK_Configure"></a>设置 Office 365 集成  
 在任何时候服务器安装完成后，可以与 Office 365 集成服务器。 如果您不 t 已有 Office 365 订阅，你可以购买一个或注册免费试订阅。  
  
 将执行以下任务：  
  
-   [第 1 步：验证 Office 365 集成要求](#BKMK_StepOne_VERIFY)  
  
-   [第 2 步：使用 Microsoft Office 365 集成服务器](#BKMK_StepTwo)  
  
-   [第 3 步：将你的组织的 Internet 域名链接到 Office 365（可选）](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a>第 1 步：验证 Office 365 集成要求  
 在开始之前，确保服务器满足以下要求：  
  
-   服务器产生任何这些操作系统：Windows Server Essentials、Windows Server Essentials 或 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 操作系统安装了 Windows Server Essentials 体验角色。  
  
-   环境仅有一台域控制器，并且你必须执行该域控制器上的 Office 365 集成。  
  
-   你必须能够服务器从连接到 Internet。  
  
-   Office 365 集成开始之前，应在服务器上安装所有关键和重要更新。  
  
-   如果你想要在电子邮件地址和 SharePoint Online 的资源使用你的组织的 Internet 域，你将需要注册域名，以便你也可以在集成链接到 Office 365 的域。 有关详细信息，请参阅[如何购买域名](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
> [!NOTE]
>  您不 t 需要提前到 Office 365 订阅。 你将能够购买的订阅或注册进行免费试用期间 Office 365 集成。 如果你 d 要看一看计划和定价的 Office 365、[比较 Office 365 计划为企业](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text)。  
  
###  <a name="BKMK_StepTwo"></a>第 2 步：使用 Microsoft Office 365 集成服务器  
 执行以下步骤，域控制器，以便与 Office 365 集成 Windows Server Essentials 服务器上。  
  
> [!NOTE]
>  该过程 s 相同是否拥有 Windows Server Essentials 或 Windows Server Essentials，但你将开始从家庭页面上的不同位置。 在 Windows Server Essentials 集成与 Office 365 和其他 Microsoft Online 服务的服务器上**服务**选项卡。在 Windows Server Essentials 执行 Office 365 集成**电子邮件**选项卡。  
  
##### <a name="to-integrate-the-server-with-office-365"></a>若要与 Office 365 集成服务器  
  
1.  登录到服务器以 administrator 身份，并打开对 Windows Server Essentials 仪表板。  
  
2.  在**家庭**页上，单击**服务**(在 Windows Server Essentials、单击**电子邮件**)，单击**使用 Microsoft Office 365 集成**，然后单击**设置 Microsoft Office 365 集成**。  
  
     使用 Microsoft Office 365 向导集成显示。  
  
3.  在**入门**页上，执行以下操作之一：  
  
    -   如果你不再拥有到 Office 365 订阅，请单击**下一步**，并按照说明进行操作以预订 Office 365 或登录到向上试订阅。  
  
         你将需要返回到向导之前登录到 Office 365。 但你不 t 需执行任何中的任务**从此处开始**Office 365 门户部分。  
  
    -   如果你已为你想要服务器上，选择与集成的 Office 365 订阅**我已经拥有的 Office 365 订阅**，然后单击**下一步**。  
  
4.  按照说明来完成向导。  
  
 已成功完成向导后，你将注意到仪表板以下更改：  
  
-   其中一个新的 s **Office 365**页面，这用于管理集成和 Office 365 订阅。  
  
-   在**用户**页上，您将查看**在线帐户**选项卡，你可以在此创建并管理使你的用户访问 Office 365 的 Microsoft Online 服务帐户。 如果你使用 re 在线交换 Windows Server Essentials 服务器，并且还，你将看到**分发组**选项卡。  
  
-   **存储**Windows Server Essentials 服务器上的页面具有**SharePoint 库**管理 SharePoint Online 库和更改权限团队站点的选项卡。 Office 365 每业务套餐包含以下基本 SharePoint Online 功能。  
  
###  <a name="BKMK_StepThree"></a>第 3 步：将你的组织的 Internet 域名链接到 Office 365（可选）  
 如果你想要使用的电子邮件发送给你的组织和 SharePoint Online 资源 Url 自己 Internet 域，可以自定义域链接到你的 Office 365 订阅。 如果你使用的 Office 365 集成 Windows Server Essentials 服务器，可以从仪表板来执行此操作。  
  
 它 s 容易执行此操作之前你创建在线, 帐户，您的用户以便批量-创建在线帐户时，你可以使用域。  
  
 当你注册 Office 365 时，你会收到域名？等*contoso*。onmicrosoft.com。如果你 d 而不是使用不同的域名？说，只需 contoso.com？可以。 你将需要购买域名，如果您不 t 已经拥有一个，并更改 DNS 记录的一部分。  
  
 自定义域为孩子设置使用与 Office 365 涉及四个任务：  
  
1.  **购买的域名。** 也就是说，注册了域注册或 DNS 宿主提供商。  
  
    -   选择适用于 Office 365 的域名。 你可以使用第二个级别域名？等 buycontoso.com，但不是第三个级别域名称？等 marketing.contoso.com。选择某个域，若要使用的 Office 365 中的详细信息，请参阅[域](https://technet.microsoft.com/library/office-365-domains.aspx)。  
  
    -   从允许所需的 Office 365 域名服务器 (DNS) 记录域注册购买它。 若要找出哪些域注册允许所需的 DNS 记录，请参阅[如何购买域名](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。 如果你已注册不同注册你的域，不必担心，域链接到 Office 365 时，您可传输到其他注册器的域。  
  
2.  **配置允许使用域名服务 Office 365 中的 DNS 记录。** 最简单方法是让域链接到你的 Office 365 订阅第 3 步中时，为你配置 DNS 记录的向导。 如果您 d 而不是自己做，请参阅[如何手动配置 DNS 记录的 Office 365 集成](#BKMK_ManuallyConfigureDNS)。  
  
3.  **将您的自定义 Internet 域链接到你的 Office 365 订阅。** 你将使用**链接到 Office 365 的域**操作。  
  
4.  **验证你的 Office 365 服务正在使用新的域名。**  
  
 如果你使用之前完成步骤 1 和 2**链接到 Office 365 的域**任务中，该向导将域名链接到 Office 365。 或者，你可以帮助你完成步骤 1 和 2 的部分或全部向导。  
  
##### <a name="to-link-your-organization-s-internet-domain-to-office-365"></a>若要将你的组织的 Internet 域链接到 Office 365  
  
1.  在仪表板上，打开**Office 365**页面，然后单击**链接到 Office 365 的域**。  
  
2.  按照说明来完成向导。  
  
     该向导可以帮助你的部分或全部注册、配置和链接一个新的或现有 Internet 域名使用 Office 365 中的步骤。  
  
     单击向导页以获取的信息，你需要完成任务上的帮助链接。 看到的某个域名称部分设置或者[Windows Server Essentials 中管理远程 Web 访问](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx)过程概述以及要求。  
  
    > [!NOTE]
    >  若要使用向导注册新域名，必须使用一个合作来提供与向导无缝集成的 Microsoft 域名称服务提供商。 若要查找域名称注册，请参阅[如何购买域名](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
3.  如果该向导将检测你的域名称并不是 t 由服务器，你将需要手动配置所需的 DNS 记录，以完成配置。 有关说明，请参阅[如何手动配置 DNS 记录的 Office 365 集成](#BKMK_ManuallyConfigureDNS)、本主题后面。  
  
4.  验证该域中使用的是 Office 365。  
  
     此处 s 小向导完成后，域名称注册验证 DNS 记录时等待。 这种情况下自动;你不再需要执行任何操作。 但通常需要大约一小时时间？和有时时间更长。 完成后，域验证时**Office 365**页面将列出你的组织的域。  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a>如何手动配置 DNS 记录的 Office 365 集成  
 如果到 Office 365 向导链接你域检测到你的域名不受服务器，以完成配置，你必须手动配置需要的域名 (DNS) 服务器记录。 在此情况下，你将找到列表中的 DNS 记录的你必须在配置**%username%\NewDNSRecords_ (n).txt**，其中*(n)*是一个随机。  
  
 下表介绍了必须添加 DNS 记录。 不同域名注册机构因条目方法可以而异。 如果你有任何问题，您的域名称注册寻求帮助。  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>将自定义 Internet 域名链接到 Office 365 所需 DNS 记录  
  
|服务|需要的 DNS 记录|目的|  
|-------------|--------------------------|-------------|  
|（多个服务）|MX| Office 365 使用此记录来验证你拥有特定的域名。 此 MX 记录不会干扰路由电子邮件。|  
|Exchange 在线|MX|提供电子邮件消息路由。 **重要提示：**是否迁移的电子邮件，未分配的零首选项 (**0**) 到新的 MX 记录。 请确保该记录的值大于分配到当前 MX 记录的值。 电子邮件迁移完成后，您就可以更改的电子邮件服务器到 Office 365，有你的偏好随意选择值新 MX 记录的重置的域名称注册。|  
|Exchange 在线|别名 (CNAME)|用于轻松地帮助的用户的自动发现记录设置 Exchange Online 和他们 Outlook 桌面客户端或移动的电子邮件客户端之间的连接。 **注意：**如果你想要使用你的组织 s 自己的域名称 (例如，http://mail.contoso.com) 而非标准 URL (https://outlook.com/owa/office365.com) 来访问 Outlook Web Access，您可以配置记录 (CName) 别名，如下所示：**类型 = CNAME，TTL = 01: 00:00，主机 = 邮件地址 = mail.office365.com**|  
|Exchange 在线|文本|指定 outlook.com，使用的 Office 365 的电子邮件服务器域有权代表你域发送电子邮件。 创建此记录，以帮助防止你站的电子邮件标记为垃圾邮件。|  
|Lync 在线|SRV|帮助使具有其他如 Windows Live 或 yahoo！的即时消息服务联盟。|  
|Lync 在线|SRV|用于轻松地帮助的用户的自动发现记录设置 Lync 桌面客户端和 Microsoft Lync 联机之间的连接。|  
  
> [!IMPORTANT]
>  完成域验证后，不要添加，或从 Office 365 门户 DNS 记录进行进一步的任何更改。  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a>下一步  
  
-   创建 Microsoft Online 服务的您的用户的帐户。  
  
     若要使用的 Office 365 服务，您的用户都必须具有与其网络用户的帐户相关联的 Microsoft Online 服务的帐户。 仪表板轻松这。 如果你重新使用新的 Office 365 订阅，你可以批量-创建在线帐户的现有的用户帐户。 如果你重新与你重新已使用可导入的用户帐户从现有联机一份 Office 365 订阅集成新服务器帐户。 有关过程，请参阅[用户管理在线帐户](Manage-Online-Accounts-for-Users.md)。  
  
> [!NOTE]
>  在 Windows Server Essentials 仪表板上的 Microsoft Online 服务的帐户被称为 Office 365 帐户。 帐户都是相同。仅术语更改。  
  
##  <a name="BKMK_ManageIntegration"></a>管理 Office 365 集成  
 与 Office 365 集成服务器后**Office 365**仪表板上的页面显示有关你的 Office 365 订阅的信息，并使这些任务可用：  
  
-   [管理你的 Office 365 订阅](#BKMK_ManageO365)？更改用于管理的订阅的管理员帐户。 打开 Office 365 管理员仪表板中管理你的订阅。  
  
-   [将你的组织的 Internet 域链接到 Office 365](#BKMK_StepThree)？如果你希望能够在发送和接收电子邮件发送给自己的域中，你可以链接到 Office 365 的域。 (早些时候，在讨论[第 3 步：将你的组织的域链接到 Office 365](#BKMK_StepThree)。)  
  
-   [禁用 Office 365 集成](#BKMK_Disable)？如果你不再希望从仪表板中管理你的 Office 365 服务、订阅和在线帐户，您可以禁用 Office 365 集成。 服务都仍然适用于 Office 365 portal。  
  
###  <a name="BKMK_ManageO365"></a>管理你的 Office 365 订阅  
 如果你需要进行更改，对你期间重新工作服务器上的 Office 365 订阅，你可以在从的 Office 365 中打开该订阅**Office 365**仪表板中的页面。 你还可以更改服务器更改 Office 365 服务会使用管理员帐户。  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>若要打开你的订阅上的 Office 365 管理员仪表板  
  
1.  在 Windows Server Essentials 仪表板上打开**Office 365**页面。  
  
2.  在**配置任务**，单击**管理 Office 365**。  
  
3.  使用你用于管理你的订阅的 Microsoft 在线帐户登录到 Office 365。  
  
     打开 Office 365 管理员仪表板。  
  
##### <a name="to-change-the-office-365-administrator-account"></a>若要更改的 Office 365 管理员帐户  
  
1.  在仪表板中，单击**Office 365**。  
  
2.  在**配置任务**，单击**更改的 Office 365 管理员帐户**。 更改管理员帐户向导将显示。 （在 Windows Server Essentials 向导名为 Office 365 管理员帐户设置。）  
  
3.  键入你想要使用连接到你的 Office 365 订阅，然后单击帐户凭据**下一步**。  
  
4.  单击**关闭**。 重启仪表板。  
  
###  <a name="BKMK_Disable"></a>禁用 Office 365 集成  
 如果你决定，你不希望从仪表板中管理你的 Office 365 服务和在线帐户，您可以禁用 Office 365 集成。 你的 Office 365 订阅将保持活动状态，并做仪表板中的任何配置更改保持有效。 例如，你将收到电子邮件发送给你已链接到你的 Office 365 订阅的域名。 你赢得 t 会丢失任何电子邮件，并且你为移动设备设置控制仍 Exchange Online 使用。  
  
 接下来，你将管理你的 Office 365 订阅、服务和 Office 365 中的资源和您的用户将需要以管理他们的在线 Office 365 帐户的密码。 不再发生密码同步时，然后禁用或删除的用户帐户不起作用用户 s 在线帐户。  
  
 本地服务器上安装的 Office 365 集成软件，因为你可以禁用该功能即使集成服务无法连接到 Office 365。  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>若要禁用与服务器的 Office 365 集成  
  
1.  在仪表板中，单击**Office 365**。  
  
2.  单击**禁用 Office 365 集成**。 显示禁用的 Office 365 向导。  
  
3.  按照说明来完成向导。  
  
> [!NOTE]
>  若要启用再次 Office 365 集成，使用**与 Office 365 集成**任务上**服务**仪表板 s 的选项卡**家庭**页面。 有关说明，请参阅[第 2 步：使用 Microsoft Office 365 集成 Windows Server Essentials 服务器](#BKMK_StepTwo)、早期本主题中。  
  
##  <a name="BKMK_Troubleshoot"></a>Office 365 集成疑难解答  
 此部分中提供的信息来帮助你解决 Windows Server Essentials 中使用的 Office 365 集成功能时，你可能会遇到的常见问题。  
  
###  <a name="BKMK_AcctsNotCreated"></a>某些 Microsoft Online 服务的帐户不被创建  
 **描述**  
  
 尝试从仪表板根本 t 成功创建 Microsoft Online 服务的一个或多个帐户。  
  
 **解决方案**  
  
1.  单击以打开包含有关未成功完成每个帐户创建请求的详细的信息的文件结果向导完成页面上的链接。 例如，的结果可能会通知你的 Microsoft Online 服务的帐户已存在请求的帐户的名称。  
  
2.  拍摄的推荐的操作来解决每个错误。  
  
3.  如果问题仍然存在，重新启动服务器，然后重新尝试创建在线帐户。  
  
###  <a name="BKMK_ProblemUninstalling"></a>卸载 Office 365 集成的问题  
 **描述**  
  
 未知出错当你尝试在禁用 Office 365 集成。  
  
 **解决方案**  
  
1.  确保计算机连接到 Internet，然后再试一次。  
  
2.  如果再次发生此错误，重启服务器，并再试一次。  
  
## <a name="see-also"></a>请参阅  
  
-   [对于 Windows Server Essentials-第 1 部分服务集成概述](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [对于 Windows Server Essentials-第 2 部分服务集成概述](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [如何使用 Microsoft Office 365 的快速入门指南](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [管理 Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
