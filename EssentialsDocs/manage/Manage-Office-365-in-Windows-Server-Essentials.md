---
title: 管理 Windows Server Essentials 中的 Office 365
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: d897c9186ff74a80d531cf84961709de54459f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433286"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 Office 365

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

将与 Microsoft Office 365 集成在 Windows Server Essentials 服务器，你可以管理 Office 365 服务和联机帐户以及从 Windows Server Essentials 仪表板的本地资源。 在本主题中，您将了解通过将你的服务器与 Office 365 集成可以获得哪些优势、 如何执行此操作，以及如何管理和 Office 365 集成故障排除。  
  
  
  
> [!IMPORTANT]
>   Office 365 集成仅支持在单个域控制器环境中。 此外，Office 365 集成向导必须在域控制器上运行。  
  
## <a name="in-this-topic"></a>本主题内容  
  
-   [为什么应该将集成 Office 365 与我的服务器？](#BKMK_IntegrationOverview)  
  
-   [设置 Office 365 集成](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [管理 Office 365 集成](#BKMK_ManageIntegration)  
  
-   [Office 365 集成故障排除](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a> 为什么应该将集成 Office 365 与我的服务器？  
 有许多充分的理由将 Office 365 与 Windows Server Essentials 服务器集成。 如果管理一些内部资源，但对于其他服务使用 Office 365，你将能够从仪表板，以及在本地资源，而不是在两个位置中管理 Office 365 服务和资源。  
  
- 管理对授予用户访问 Office 365 以及用户帐户的联机帐户：  
  
  -   在单个步骤中为用户创建 Microsoft Online Services 帐户，或在服务器上为现有联机帐户创建用户帐户。 你还可以向新的或现有的用户帐户添加联机帐户。  
  
       从仪表板创建联机帐户时，用户登录到 Office 365 与它们在服务器使用相同的密码。 如果用户更改该用户帐户的密码，则联机密码也将随之更改。 而且你知道他们的联机帐户密码始终满足你为用户帐户设置的安全要求。  
  
  -   在用户帐户的整个生命周期内，将联机帐户与用户帐户一起管理。 如果停用用户帐户，则联机帐户也将停用。 如果删除用户帐户，则联机帐户也将随之删除。  
  
  -   在 Windows Server Essentials 服务器上，还管理电子邮件的 Exchange Online 通讯组。  
  
- 发送和接收来自你组织的 Internet 域 (例如，contoso.com) 电子邮件，方法是将自定义 Internet 域链接到 Office 365 订阅。  
  
- 从仪表板管理你的订阅和 Office 365 集成。  
  
- 如果你的订阅包括 SharePoint Online 库中，Windows Server Essentials 服务器与 Office 365 集成，您可以：  
  
  - 创建和管理 SharePoint Online 库从仪表板。  
  
    > [!NOTE]
    >  您还可以使用 My Server 2012 R2 应用从便携式计算机、 移动设备或 Windows phone SharePoint Online 库中的文档处理。 此功能才适用于 Windows Server Essentials。 有关详细信息，请参阅[使用 My Server App](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。  
  
  - 从仪表板中，更改为 SharePoint Online 团队网站权限或从仪表板以进行其他更改打开团队网站。  
  
    有关详细信息，请参阅[管理 SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md)。  
  
- 如果你订阅 Exchange online，可以使用 Windows Server Essentials 服务器与 Office 365 集成管理的移动设备的用户用来连接到公司电子邮件服务器：  
  
  -   当移动设备连接到公司电子邮件服务器时，需要密码保护。 设置最小密码长度、允许登录尝试失败的次数，以及两次登录尝试之间所需的最短时间。  
  
  -   如果你知道该设备型号存在安全问题，请阻止此类移动设备连接到 Exchange Online。  
  
  -   如果移动设备丢失或被盗，请擦除该设备，以在该设备下次被打开时删除任何敏感的公司数据。  
  
- Office 365 集成为你提供了连接到 Office 365 服务和资源的新方法：  
  
  -   从 Windows Server Essentials 快速启动板打开 Office 365 服务。 有关信息，请参阅[Quick Start Guide to Using Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)。  
  
  -   使用 My Server 2012 R2 应用从便携式计算机、 移动设备或 Windows phone SharePoint Online 库中的文档处理。 有关信息，请参阅[使用 My Server App](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)。 此功能才在 Windows Server Essentials 中可用。  
  
##  <a name="BKMK_Configure"></a> 设置 Office 365 集成  
 在任何时间服务器安装完成后，可以与 Office 365 集成您的服务器。 如果还没有 Office 365 订阅，可以购买一个或注册免费试用版订阅。  
  
 你将执行以下任务：  
  
-   [步骤 1：验证 Office 365 集成要求](#BKMK_StepOne_VERIFY)  
  
-   [步骤 2：将服务器与 Microsoft Office 365 集成](#BKMK_StepTwo)  
  
-   [步骤 3：将组织的 Internet 域名链接到 Office 365 （可选）](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a> 步骤 1:验证 Office 365 集成要求  
 在开始之前，请确保服务器满足以下要求：  
  
-   服务器可以具有以下任一操作系统：Windows Server Essentials、 Windows Server Essentials 或 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter 操作系统安装了 Windows Server Essentials 体验角色。  
  
-   在环境只能有一个域控制器，并且必须在域控制器上执行 Office 365 集成。  
  
-   你必须能够从服务器连接到 Internet。  
  
-   启动 Office 365 集成之前，应在服务器上安装所有关键和重要更新。  
  
-   如果你想要使用电子邮件地址和使用 SharePoint Online 资源的组织的 Internet 域，您将需要注册的域名，以便可以在集成期间将该域链接到 Office 365。 有关详细信息，请参阅 [如何购买域名](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
> [!NOTE]
>  不需要提前订阅 Office 365。 你可以购买订阅或注册免费试用版在 Office 365 集成的过程。 如果你想要看一看的 Office 365 计划和定价[比较面向企业的 Office 365 计划](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text)。  
  
###  <a name="BKMK_StepTwo"></a> 步骤 2:将服务器与 Microsoft Office 365 集成  
 若要将与 Office 365 集成在 Windows Server Essentials 服务器的域控制器上执行以下过程。  
  
> [!NOTE]
>  是否有 Windows Server Essentials 或 Windows Server Essentials 中，但你会从主页页面上的不同位置开始，该过程的相同。 在 Windows Server Essentials 集成与 Office 365 和其他 Microsoft Online Services 的服务器上**Services**选项卡。在 Windows Server Essentials 中，对 Office 365 集成**电子邮件**选项卡。  
  
##### <a name="to-integrate-the-server-with-office-365"></a>将服务器与 Office 365 集成  
  
1. 作为管理员，在服务器上登录并打开 Windows Server Essentials 仪表板。  
  
2. 上**主页**页上，单击**Services** (在 Windows Server Essentials 中，单击**电子邮件**)，单击**与 Microsoft Office 365 集成**，然后单击**设置 Microsoft Office 365 集成**。  
  
    将出现“与 Microsoft Office 365 集成”向导。  
  
3. 在“入门”  页上，执行以下操作之一：  
  
   -   如果没有 Office 365 订阅，请单击**下一步**，并按照说明订阅 Office 365 或注册了试用版订阅。  
  
        你将需要登录到 Office 365，然后返回到向导。 但无需执行的任务中的任何**从这里开始**Office 365 门户的一部分。  
  
   -   如果已有对想要将服务器上，选择与集成的 Office 365 订阅**我已经拥有 Office 365 订阅**，然后单击**下一步**。  
  
4. 按照说明完成向导。  
  
   在向导成功完成后，您会注意到对仪表板的以下更改：  
  
-   新增了一个**Office 365**页上，用于管理集成和你的 Office 365 订阅。  
  
-   上**用户**页上，将看到**联机帐户**选项卡可以在其中创建和管理那些授予用户访问 Office 365 的 Microsoft Online Services 帐户。 如果你使用 Exchange Online 并具有 Windows Server Essentials 服务器，还会看到**通讯组**选项卡。  
  
-   **存储**Windows Server Essentials 服务器上的页具有**SharePoint 库**选项卡用于管理 SharePoint Online 库和更改团队网站的权限。 适用于 Office 365 的每个业务计划包括这些基本的 SharePoint Online 功能。  
  
###  <a name="BKMK_StepThree"></a> 步骤 3:将组织的 Internet 域名链接到 Office 365 （可选）  
 如果你想要使用你自己的 Internet 域电子邮件发送到你的组织和 SharePoint Online 资源的 Url 中，您可以将自定义域链接到 Office 365 订阅。 如果与 Office 365 集成在 Windows Server Essentials 服务器，可以从仪表板来执行此操作。  
  
 它是你为用户创建联机帐户，因此当你批量创建联机帐户时，可以使用域之前执行此操作最简单的方法。  
  
 注册 Office 365 时获得一个域名？ 例如， *contoso*.onmicrosoft.com。 如果您更愿意使用不同的域名？ 说，只需 contoso.com？ 你可以。 你将需要购买一个域名，如果没有已拥有其中一个，并更改一些 DNS 记录。  
  
 设置自定义域要用于 Office 365 涉及四个任务：  
  
1. **购买一个域名。** 即，通过域注册机构或 DNS 托管提供者对其进行注册。  
  
   -   选择适用于 Office 365 的域名。 可以使用第二级域名？ 例如 buycontoso.com？ 但第三级域名称不？ 例如，marketing.contoso.com。 有关选择要在 Office 365 中使用的域的详细信息，请参阅[域](https://technet.microsoft.com/library/office-365-domains.aspx)。  
  
   -   从允许所需的 Office 365 域名服务器 (DNS) 记录的域注册机构购买它。 若要查明哪些域注册机构允许所需的 DNS 记录，请参阅 [如何购买域名](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。 如果你已使用其他注册机构注册域，也不必担心;将域链接到 Office 365 时，可以传输到其他注册机构的域。  
  
2. **配置允许 Office 365 服务使用域名的 DNS 记录。** 最简单方法是，让向导为你配置的 DNS 记录时你将域链接到你在步骤 3 中的 Office 365 订阅。 如果您想执行操作它自己，请参阅[如何手动配置 DNS 记录用于 Office 365 集成](#BKMK_ManuallyConfigureDNS)。  
  
3. **自定义 Internet 域链接到你的 Office 365 订阅。** 将使用**将域链接到 Office 365**操作。  
  
4. **验证 Office 365 服务使用新域名。**  
  
   如果完成步骤 1 和 2，然后使用**将域链接到 Office 365**任务中，该向导可以将域名链接到 Office 365。 或者，你可以借助向导完成步骤 1 和步骤 2 的部分或全部操作。  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>若要将组织的 Internet 域链接到 Office 365  
  
1.  在仪表板上打开“Office 365”  页，然后单击“将域链接到 Office 365”  。  
  
2.  按照说明完成向导。  
  
     该向导可以帮助你完成部分或全部注册、 配置和链接新的或现有 Internet 域名使用 Office 365 中的步骤。  
  
     在向导页上单击帮助链接，以获取完成某个任务所需的信息。 或参阅域名称部分设置[Windows Server Essentials 中管理远程 Web 访问](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx)过程概述和要求。  
  
    > [!NOTE]
    >  若要使用该向导注册新的域名，则必须使用一个与 Microsoft 合作的域名服务提供商，以提供与该向导的无缝集成。 若要查找域名注册机构，请参阅 [如何购买域名](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660)。  
  
3.  如果向导检测到你的域名不由服务器管理，你将需要手动配置所需的 DNS 记录，以完成配置。 有关说明，请参阅本主题后面的 [如何手动配置用于 Office 365 集成的 DNS 记录](#BKMK_ManuallyConfigureDNS)。  
  
4.  验证在 Office 365 中正在使用该域。  
  
     向导完成后，虽然域名注册机构验证 DNS 记录后需等待片刻。 发生这种情况自动保存功能。您无需执行任何操作。 但它通常需要大约一小时？ 和有时稍长的时间。 域验证完成后， **Office 365**页将列出你组织的域。  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a> 如何手动配置用于 Office 365 集成的 DNS 记录  
 如果“将域链接到 Office 365”向导检测到你的域名未由服务器托管，此时若要完成配置，必须手动配置所需的域名服务器 (DNS) 记录。 在这种情况下，会发现一组必须在配置的 DNS 记录 **%username%\NewDNSRecords_ (n).txt**，其中 *(n)* 是一个随机数。  
  
 下表说明了必须添加的 DNS 记录。 输入方法可能因不同的域名注册机构而有所不同。 如果有任何疑问，请向你的域名称注册机构寻求帮助。  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>将自定义 Internet 域名链接到 Office 365 所需的 DNS 记录  
  
|服务|所需的 DNS 记录|用途|  
|-------------|--------------------------|-------------|  
|（多个服务）|MX| Office 365 使用此记录验证你拥有特定的域名。 此 MX 记录不会干扰电子邮件路由。|  
|Exchange Online|MX|提供电子邮件路由。 **重要说明：** 如果要迁移电子邮件，请不要将零 (**0**) 作为首选项分配给新的 MX 记录。 请确保该记录的值大于分配给当前 MX 记录的值。 电子邮件迁移完成后，已准备好将电子邮件服务器更改为 Office 365，具有你的域名注册机构重置新 MX 记录的首选项值。|  
|Exchange Online|别名 (CNAME)|自动发现用于帮助用户在 Exchange Online 及其 Outlook 桌面客户端或移动电子邮件客户端之间轻松地设置连接的记录。 **注意：** 如果想要通过使用组织自己的域名访问 Outlook Web Access (例如， http://mail.contoso.com)而不是标准的 URL (https://outlook.com/owa/office365.com)，可以配置别名 (CName) 记录，如下所示：**Type=CNAME, TTL=01:00:00, HostName=mail, Address=mail.office365.com**|  
|Exchange Online|TXT|指定该 outlook.com、 Office 365 电子邮件服务器使用的域有权代表你的域发送电子邮件。 创建此记录，以帮助防止你的出站电子邮件被标记为垃圾邮件。|  
|Lync Online|SRV|帮助启用与其他即时消息服务（例如 Windows Live 或 yahoo!）的联盟。|  
|Lync Online|SRV|自动发现用于帮助用户在 Lync 桌面客户端和 Microsoft Lync Online 之间轻松地设置连接的记录。|  
  
> [!IMPORTANT]
>  域验证完成后，不要尝试添加或从 Office 365 门户对 DNS 记录进行任何进一步的更改。  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a> 下一步  
  
-   为用户创建 Microsoft Online Services 帐户。  
  
     若要使用 Office 365 服务，用户必须具有与其网络用户帐户相关联的 Microsoft Online Services 帐户。 仪表板使此操作变得简单。 如果您使用的是新的 Office 365 订阅，您可以批量创建联机帐户为现有用户帐户。 如果你已在使用 Office 365 订阅随附集成新的服务器，则可以从现有的联机帐户导入用户帐户。 有关过程，请参阅[管理的用户的联机帐户](Manage-Online-Accounts-for-Users.md)。  
  
> [!NOTE]
>  Windows Server Essentials 中仪表板的 Microsoft Online Services 帐户称为 Office 365 帐户。 这两个帐户是相同的；只有术语发生了更改。  
  
##  <a name="BKMK_ManageIntegration"></a> 管理 Office 365 集成  
 将你的服务器与 Office 365 集成后**Office 365**仪表板页显示有关你的 Office 365 订阅的信息，可执行这些任务：  
  
-   [管理你的 Office 365 订阅](#BKMK_ManageO365)？更改用于管理订阅的管理员帐户。 打开 Office 365 管理仪表板管理你的订阅。  
  
-   [组织的 Internet 域链接到 Office 365](#BKMK_StepThree) ？如果你想要能够发送和接收电子邮件发送给你自己的域，可以将该域链接到 Office 365。 (前面[步骤 3:你的组织域链接到 Office 365](#BKMK_StepThree)。)  
  
-   [禁用 Office 365 集成](#BKMK_Disable)？如果不想从仪表板管理 Office 365 服务、 订阅和联机帐户，则可以禁用 Office 365 集成。 服务是在 Office 365 门户上仍然可用。  
  
###  <a name="BKMK_ManageO365"></a> 管理你的 Office 365 订阅  
 如果需要对你的 Office 365 订阅进行更改，在服务器上工作时，可以在 Office 365 中打开订阅**Office 365**仪表板页。 此外可以更改服务器用于对 Office 365 服务进行更改的管理员帐户。  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>在 Office 365 管理仪表板上打开订阅  
  
1.  在 Windows Server Essentials 仪表板上打开**Office 365**页。  
  
2.  在“配置任务”  中，单击“管理 Office 365”  。  
  
3.  用于管理你的订阅的 Microsoft 联机帐户登录到 Office 365。  
  
     Office 365 管理仪表板将打开。  
  
##### <a name="to-change-the-office-365-administrator-account"></a>更改 Office 365 管理员帐户  
  
1.  在仪表板上，单击“Office 365”  。  
  
2.  在“配置任务”  中，单击“更改 Office 365 管理员帐户”  。 将出现“更改管理员帐户”向导。 （在 Windows Server Essentials 中，该向导名为设置 Office 365 管理员帐户。）  
  
3.  键入你想要使用以连接到 Office 365 订阅，然后单击该帐户的凭据**下一步**。  
  
4.  单击 **“关闭”** 。 仪表板将重新启动。  
  
###  <a name="BKMK_Disable"></a> 禁用 Office 365 集成  
 如果您不想从仪表板管理你的 Office 365 服务和联机帐户，则可以禁用 Office 365 集成。 你的 Office 365 订阅将保持活动状态，并从仪表板进行任何配置更改将继续有效。 例如，你将收到电子邮件地址为链接到你的 Office 365 订阅的域名。 不会丢失任何电子邮件，并为移动设备设置的控件仍是使用 Exchange Online。  
  
 展望未来，将管理 Office 365 订阅、 服务和 Office 365 中的资源和你的用户将需要管理 Office 365 中其联机帐户的密码。 密码同步不会再发生，并且禁用或删除用户帐户不会影响用户的联机帐户。  
  
 由于 Office 365 集成软件安装在本地服务器上，你可以禁用该功能，即使集成服务无法连接到 Office 365。  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>禁用与服务器的 Office 365 集成  
  
1.  在仪表板上，单击“Office 365”  。  
  
2.  单击“禁用 Office 365 集成”  。 将出现“禁用 Office 365”向导。  
  
3.  按照说明完成向导。  
  
> [!NOTE]
>  若要启用再次 Office 365 集成，请使用**与 Office 365 集成**上的任务**服务**仪表板的选项卡**主页**页。 有关说明，请参阅[步骤 2:将 Windows Server Essentials 服务器与 Microsoft Office 365 集成](#BKMK_StepTwo)本主题中前面部分。  
  
##  <a name="BKMK_Troubleshoot"></a> Office 365 集成故障排除  
 本部分提供信息以帮助您解决在 Windows Server Essentials 中使用 Office 365 集成功能时可能遇到的常见问题。  
  
###  <a name="BKMK_AcctsNotCreated"></a> 未创建某些 Microsoft Online Services 帐户  
 **说明**  
  
 从仪表板创建一个或多个 Microsoft Online Services 帐户的尝试未成功。  
  
 **解决方案**  
  
1.  单击向导完成页上的链接以打开结果文件，该文件包含未成功完成的每个帐户创建请求的详细信息。 例如，某条结果可能会告诉你，具有请求的帐户名称的 Microsoft Online Services 帐户已存在。  
  
2.  请执行推荐的操作来解决每个错误。  
  
3.  如果此问题仍然存在，请重新启动服务器，然后尝试重新创建联机帐户。  
  
###  <a name="BKMK_ProblemUninstalling"></a> 出现卸载 Office 365 集成问题  
 **说明**  
  
 在尝试禁用 Office 365 集成时，将出现未知的错误。  
  
 **解决方案**  
  
1.  确保计算机已连接到 Internet，然后重试。  
  
2.  如果再次发生该错误，请重新启动服务器，然后重试。  
  
## <a name="see-also"></a>请参阅  
  
-   [适用于 Windows Server Essentials-第 1 部分的服务集成概述](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [适用于 Windows Server Essentials-第 2 部分的服务集成概述](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [使用 Microsoft Office 365 的快速入门指南](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Manage Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
