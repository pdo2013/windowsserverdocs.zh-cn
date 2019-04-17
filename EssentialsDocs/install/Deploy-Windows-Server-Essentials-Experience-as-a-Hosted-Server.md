---
title: "部署 Windows Server Essentials 体验作为托管服务器"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c299d2b5f3d6b48693c473754a5205a7d26b5d6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>部署 Windows Server Essentials 体验作为托管服务器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本文档包括特定于想要将 Microsoft Windows Server 16 部署在其实验中安装了 Windows Server Essentials 体验角色（也称为 Windows Server Essentials 文档的其余部分），并且想要为其客户即服务提供 Windows Server Essentials 体验的宿主的信息。 本文包含以下部分：  
  

-   [Windows Server 软件包体验概述](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [装载 Windows Server Essentials 体验的权益](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [受支持的部署选项](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [受支持的网络拓扑](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [自定义 Windows Server Essentials 体验角色的图像](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [自动部署 Windows Server Essentials 体验](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [将数据从 Windows 小型企业服务器迁移到 Windows Server Essentials 体验](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [使用 Windows PowerShell 执行常见任务](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [与 Windows Server Essentials 的电子邮件集成](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [监视和管理通过本机的工具](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [测试的方案](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [支持信息](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Windows Server 软件包体验概述](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [装载 Windows Server Essentials 体验的权益](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [受支持的部署选项](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [受支持的网络拓扑](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [自定义 Windows Server Essentials 体验角色的图像](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [自动部署 Windows Server Essentials 体验](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [将数据从 Windows 小型企业服务器迁移到 Windows Server Essentials 体验](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [使用 Windows PowerShell 执行常见任务](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [与 Windows Server Essentials 的电子邮件集成](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [监视和管理通过本机的工具](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [测试的方案](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [支持信息](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a>Windows Server 软件包体验概述  
 Windows Server Essentials 体验还处于适用于 Windows Server 2012 R2 Standard 和 Windows Server 2012 R2 Datacenter server 角色。 Windows Server Essentials 体验角色已安装在运行 Windows Server 2012 R2 的服务器上，当客户可以利用适用于 Windows Server Essentials 不显示锁和限制的所有功能。 Windows Server Essentials 体验使中小型企业为以下跨本地解决方案：  
  
-   **数据存储和保护**可以存储客户"在中心位置美分的数据和备份 server 和网络中的客户端计算机 (小于 75) 的保护服务器和客户端的数据。  
  
-   **用户管理**可以管理的用户和组通过简化的服务器仪表板。 此外，与 Microsoft Azure Active Directory (Azure AD) 集成实现轻松数据访问 Microsoft 联机服务（例如，Office 365、Exchange Online 和 SharePoint Online）的用户使用他们域的凭据。  
  
-   **服务集成**可与 Microsoft 的联机服务（如 Office 365、SharePoint Online 和 Microsoft Azure 备份）集成服务器。 你还可以将服务器集成服务或服务提供的第三方提供商。  
  
-   **随时随地访问**客户可以访问 server、网络计算机和数据从任何地方它们具有 Internet 连接和使用几乎所有设备。 远程访问 Web 使他们访问的应用程序和更流畅、触摸体验更加友好的浏览器体验的数据。 我服务器应用可使他们能够从 Windows Phone 或 Microsoft 官方商城应用访问数据。  
  
-   **媒体流式传输**最终客户如果你在与 Windows Server Essentials 体验启用的服务器上安装媒体程序包，可以将音乐、视频和照片存储在共享文件夹，然后从联网的计算机或访问远程 Web 访问这些媒体文件。  
  
-   **健康监视**可以监视器网络健康并获取自定义的运行状况的报告。  
  
##  <a name="BKMK_Benefits"></a>装载 Windows Server Essentials 体验的权益  
  Windows Server 软件包体验是 Windows Server 中的角色，因此你可以重复使用现有的部署和 Windows Server 部署和配置 Windows Server Essentials 体验角色中的管理框架。 举办的 Windows Server Essentials 体验角色提供以下优势：  
  
-   **简化部署**只需打开 Windows Server Essentials 体验角色，一些最常用的角色和功能已打开并配置的最佳实践中小型企业。 你可以自定义 Windows Server Essentials 功能，或是隐藏一些本地功能。 如果你使用的 Windows Azure 包，你可以在 Windows Server 2012 R2 上的 Windows Server Essentials 体验下载库模板。  
  
-   **简化仪表板**Windows Server Essentials 仪表板简化了如管理 server 文件夹、服务器存储、备份和还原、用户或组帐户、设备、远程访问和电子邮件的常见任务。 中小型业务客户可以执行日常的管理任务，而不是呼叫帮助 Desk 技术支持。  
  
-   **扩展性**的 Windows Server Essentials 仪表板和 Windows Server Essentials 接头软件进行扩展。 你可以添加自己品牌并集成，提供服务，以便你的客户具有一个入口点所有有关其服务器和服务。  
  
-   **监视器**新版本的系统中心监视包适用于监视器和管理多个运行 Windows Server Essentials 服务器。 若要下载管理包，请参阅[适用于 Windows Server Essentials 的系统中心管理包](https://www.microsoft.com/download/details.aspx?id=40809)。  
  
##  <a name="BKMK_SupportedDeployment"></a>受支持的部署选项  
  Windows Server 可以为域控制器在新的 Active Directory 环境; 部署 essentials 体验或者，它可以部署到作为域成员现有的 Active Directory 环境。  
  
 我们建议你先部署 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter，并安装 Windows Server Essentials 体验角色。 使用此部署方法，你将收到 Windows Server Essentials 版本，不显示锁和限制的所有功能。  
  

 有关与 Windows Server Essentials 体验角色安装 Windows Server 2012 R2 的详细信息，请参阅[安装和配置 Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。  


  
##  <a name="BKMK_SupportedToplogy"></a>受支持的网络拓扑  
 若要使用 Windows Server Essentials 体验从漫游客户端，应启用 VPN。 若要从漫游客户端，服务器启用远程访问，你需要以打开端口 443 和端口 80 服务器上。  
  
 下面是两种典型服务器端网络拓扑，以及如何可能配置 VPN 并远程访问 Web:  
  
-   **1 拓扑**（这是首选的拓扑，和它的地方的服务器和 VPN IP 范围相同子网）：  
  
    -   设置下网络地址翻译 (NAT) 设备的一个单独的虚拟网络中的服务器。  
  
    -   启用 DHCP 服务中的虚拟网络或分配服务器的静态 IP 地址。  
  
    -   向前公共 IP 443 路由器端口到本地网络的服务器的地址。  
  
    -   允许端口 443 VPN 直通。  
  
    -   设置为服务器地址相同子网覆盖范围内 VPN IPv4 地址池。  
  
    -   指定第二个服务器内相同子网，但退出 VPN 地址池的静态 IP 地址。  
  
-   **拓扑 2**:  
  
    -   指定服务器专用的 IP 地址。  
  
    -   允许端口 443 服务器到达公共端口 443 IP 地址。  
  
    -   允许端口 443 VPN 直通。  
  
    -   指定不同范围 VPN IPv4 地址池和服务器的地址。  
  
 拓扑 2 服务器的第二个场景均不支持因为到同一个域，不能添加另一台服务器。  
  
 你可以通过使用我们的 Windows PowerShell 脚本无人照看部署期间启用 VPN 或它可以设置使用向导后在初始配置。  
  
 若要使用 Windows PowerShell 启用 VPN，使用管理权限运行以下命令，在运行 Windows Server Essentials 服务器上，并提供所有必需的信息。  
  
```  
##  
## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
##  
  
$myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
$mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
$mySslCertificatePassword = ConvertTo-SecureString  œAsPlainText  œForce '******';   ## password for private key of the SSL certificate  
$skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
Set-WssDomainNameConfiguration  œDomainName $myExternalDomainName  œCertificatePath $mySslCertificateFile  œCertificateFilePassword $mySslCertificatePassword  œNoCertificateVerification  
##  
## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
##  
  
Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
  
```  
  
> [!NOTE]
>  如果你无法在前的客户拥有服务器提供 VPN 连接，，确保服务器端口 3389 可通过 Internet 连接，以便客户可以使用远程桌面协议连接到服务器，并对其进行配置。  
  
##  <a name="BKMK_CustomizeImage"></a>自定义 Windows Server Essentials 体验角色的图像  
 您可以配置 Windows Server Essentials 体验角色之前自定义映像。 若要了解详细信息的标准 Windows Server Sysprep 过程，请参阅[Windows 评估和部署工具包](https://msdn.microsoft.com/library/hh825420.aspx)。 你准备使用 Sysprep 映像后，你可以使用它，或新部署重新封装 Install.wim 插入。  
  
 如果你使用虚拟机管理器，你可以使用运行实例创建模板。 此过程使用 Sysprep 准备实例，并关闭计算机。 库中存储模板后，你可以通过案例基础上使用它。  
  
 安装 Windows Server Essentials 体验角色后，你可以自定义 Windows Server Essentials 中的功能。 最重要的自定义项之一是**IsHosted**注册表项：**HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \Deployment\IsHosted**。  
  
 如果此项为 0x1 设置，某些本地功能将更改行为。 这些功能更改包括：  
  
-   **客户端备份**将新加入的客户端计算机的默认情况下关闭客户端备份。  
  
-   **客户端还原服务**将禁用客户端恢复服务，并且 UI 将从仪表板中隐藏。  
  
-   **文件历史记录**创建新的用户帐户设置文件历史记录不会自动管理服务器。  
  
-   **服务器备份**将禁用服务器备份服务，并从仪表板，将会隐藏服务器备份 UI。  
  
-   **存储空间**仪表板中，将会隐藏创建或管理存储空间的 UI。  
  
-   **随时随地访问**设置任何地方访问向导运行时，将默认情况下跳过的路由器并 VPN 配置。  
  
 如果你想要控制列出的每个功能的行为，你可以为每个设置相应的注册表项。 有关如何注册表项以信息，请参阅[自定义和部署 Windows Server Essentials 在 Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="BKMK_AutomateDeployment"></a>自动部署 Windows Server Essentials 体验  
 若要自动部署，你需要首先部署操作系统，然后安装了 Windows Server Essentials 体验角色。  
  
-   若要自动部署 Windows Server 2012 R2 Standard 或 Windows Server 2012 R2 Datacenter，按照说明进行操作，并在[Windows 评估和部署工具包](https://msdn.microsoft.com/library/hh825420.aspx)。  
  
-   若要了解如何使用 Windows PowerShell 安装 Windows Server Essentials 体验角色，请参阅[安装和配置 Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx)。  
  
> [!NOTE]
>  确保的主机虚拟机和 Windows Server Essentials 体验的时区的区域设置都是相同。 否则，你可能会遇到几个错误。 这些功能包括：初始配置服务器的可能不会上成功证书相关的任务，证书可能无法工作的几个小时后安装 Windows Server Essentials 体验角色，以及不会正确更新的设备的信息。  
  
 部署后，使用 Windows PowerShell cmdlet**获取 WssConfigurationStatus**验证初始配置是否成功。 返回的状态应该下列情况之一：**Notstarted**，**FinishedWithWarning**，**运行**，**已完成**，**失败**，或**PendingReboot**。  
  
 在初始配置时，将重新启动的服务器。 如果你需要以防止自动重新启动，你可以使用以下命令添加注册表项，然后开始在初始配置：  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 在初始配置启动后，你可以使用**获取 WssConfigurationStatus**检查初始配置的状态，且状态时**PendingReboot**，你可以重新启动你的服务器。  
  
##  <a name="BKMK_Migrate"></a>将数据从 Windows 小型企业服务器迁移到 Windows Server Essentials 体验  
 你可以从运行 Windows 小型企业服务器 2011 年、小型企业的 Windows Server 2008、Windows 小型企业 Server 2003 或 Windows Server Essentials 到运行 Windows Server Essentials 服务器服务器迁移数据。 查看[迁移到 Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)迁移指南本地 2migrations 并进行必要的自定义基于你宿主环境。  
  
> [!NOTE]
>  建议你将在相同子网源服务器和目的地的服务器。 如果不能，你应确保：  
>   
>  -   源服务器和目的地服务器都可以访问彼此"美分 s 内部 DNS 姓名。  
> -   打开了所有所需的端口。  
  
 迁移后，你可以升级你许可证去锁和限制。 有关详细信息，请参阅[从 Windows Server Essentials 转换为 Windows Server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx)。  
  
##  <a name="BKMK_PowerShell"></a>使用 Windows PowerShell 执行常见任务  
 此部分中介绍的一些你可以通过使用 Windows PowerShell 执行常见任务。  
  
### <a name="enable-remote-web-access"></a>允许远程访问  
 **语法**:  
  
 Enable-WssRemoteWebAccess[-SkipRouter][-DenyAccessByDefault][-ApplyToExistingUsers]  
  
 **示例**:  
  
 $Enable-WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers  
  
 此命令将与路由器配置自动启用远程 Web 访问和更改现有的所有用户的默认的访问权限。  
  
### <a name="add-user"></a>添加用户  
 **语法**:  
  
 Add-WssUser[的姓名] < string\ > [-密码] < securestring\ > [-AccessLevel < string\ > {用户 & #124;管理员}] [的名字 < string\ >] [-姓氏 < string\ >] [-AllowRemoteAccess] [-AllowVpnAccess] < CommonParameters\ >  
  
 **示例**:  
  
 $password = ConvertTo-SecureString"Passw0rd！" -asplaintext œforce$ 添加-WssUser-名称 User2Test-密码 $password Accesslevel 管理员-名字用户 2 姓氏测试  
  
 此命令将使用密码 Passw0rd 命名 User2Test 管理员 ！。  
  
### <a name="add-server-folder"></a>添加服务器文件夹  
 **语法**:  
  
 Add-WssFolder[的姓名] < string\ > [-路径] < string\ > [[-描述] < string\ >] [-KeepPermissions] < CommonParameters\ >  
  
 **示例**:  
  
 $添加 WssFolder-名称"MyTestFolder"-路径"C:\ServerFolders\MyTestFolder"  
  
 此命令将添加服务器文件夹名为 MyTestFolder 指定位置。  
  
##  <a name="BKMK_EmailIntegration"></a>与 Windows Server Essentials 的电子邮件集成  
 你可以与 Office 365 集成 Windows Server Essentials 体验或托管 Exchange Server。 如果你希望你客户使用托管的电子邮件，你需要开发外接程序集成托管的电子邮件解决方案与 Windows Server Essentials 体验。 有关详细信息，请参阅[Windows Server Essentials SDK](https://msdn.microsoft.com/library/gg513877.aspx)。  
  
##  <a name="BKMK_Monitoring"></a>监视和管理通过本机的工具  
 此部分中讨论了适用于 Windows Server 2012 R2 到监视器和管理服务器的原始工具。  
  
### <a name="group-policy"></a>组策略  
  Windows Server 软件包体验利用 Windows Server 2012 R2 在本地组策略支持，并提供配置文件夹重定向和安全设置的用户界面。  
  
> [!NOTE]
>  在托管的环境中，如果用户配置文件的文件夹重定向处于启用状态，它会增加最终用户登录的数据大小大时的时间。  
  
### <a name="system-center-monitoring-pack"></a>System Center 监控包  
 Windows Server Essentials 体验 system Center 监视包监视健康警报系统，以帮助你管理大量运行 Windows Server Essentials 专用于小型企业组织的服务器。 有关详细信息，请参阅[适用于 Windows Server Essentials 的系统中心管理包](https://www.microsoft.com/download/details.aspx?id=40809)。  
  
### <a name="backup-and-restore"></a>备份和还原  
  Windows Server 2012 对 Windows Server Essentials 体验 R2 允许你备份 server 和网络中的客户端计算机。  
  
#### <a name="server-backup"></a>服务器备份  
 Windows Server 软件包支持备份服务器两种方法：本地备份并关闭本地备份。 如果你想要将你自己的服务器备份解决方案的部署，你可以自定义这些选项。  
  
-   **本地备份**允许你为单独磁盘定期执行阻止级别增量备份。 作为宿主，可以连接到运行 Windows Server Essentials 虚拟机虚拟硬盘，然后配置服务器备份到该虚拟硬盘。 在运行 Windows Server Essentials 虚拟机比不同的物理磁盘应位于虚拟硬盘。  
  
    > [!NOTE]
    >  如果你有虚拟机备份的其他解决方案，并且你不希望你的用户，若要查看 Windows Server Essentials 本机 Server 备份功能，你可以将其关闭，并从仪表板中删除相关的用户界面。 有关详细信息，请参阅[自定义服务器备份](https://technet.microsoft.com/library/dn293413.aspx)部分[自定义和部署 Windows Server Essentials 在 Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)。  
  
-   **本地关闭备份**允许你定期备份到的基于云的服务的服务器数据。 你可以下载并安装 Windows Server Essentials，来充分利用由 Microsoft Azure 备份 Microsoft Azure 备份集成模块。  
  
     有关详细信息，请参阅集成 Windows Azure 备份与 Windows Server Essentials 中的部分[管理服务器备份](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md)。  
  
     如果你或你的用户喜欢另一个云服务，您应该考虑以下各项：  
  
    -   更新仪表板的用户界面，以便它链接到而不是默认 Azure 备份到你首选的云服务。  
  
    -   （可选）开发外接程序仪表板配置和管理云备份服务。  
  
#### <a name="client-computer-backup"></a>客户端计算机备份  
 Windows Server 软件包体验支持两种类型的客户端数据备份：完整客户端备份和文件历史记录。  
  
> [!NOTE]
>  备份客户可能会影响性能，因为数据所需的客户端从到服务器通过传输 VPN。  
  
##### <a name="full-client-backup"></a>完整客户端备份  
 默认情况下连接到 Windows Server Essentials 网络的客户端设备的情况下，完全客户端备份处于打开状态。 它完全为客户端将备份数据和系统信息，并支持数据消除。 将运行 Windows Server Essentials 的服务器上存储的备份数据。 这使失败客户端从之前的备份点检索数据。  
  
 一些注意事项完整的客户端计算机备份是：  
  
-   **性能**初始客户端备份可能需要较长时间由于要上载的数据量。  
  
-   **稳定性**有时 Internet 连接不稳定在客户端。 客户端备份旨在自动，继续和客户端备份数据库创建一个检查点每次 40 GB 的数据的备份。 如果你希望不可靠的 Internet 连接，你可以更改此值为小的数字。  
  
    -   若要启用检查点作业：在服务器上，设置注册表项**HKLM\Software\ Microsoft \ Windows Server \Backup\GetCheckPointJobs**为 1。  
  
    -   若要更改检查点 threshold：在客户端，更改**HKLM\Software\ Microsoft \ Windows Server \Backup\CheckPointThreshold**从其默认值为 40 GB。  
  
-   **客户端空白的基本金属还原**因为 Windows 预安装环境 VPN 连接不支持，客户端空白的基本金属还原不受支持。 你应该通过中的步骤隐藏客户端还原服务任务[自定义和部署 Windows Server Essentials 在 Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)。  
  
##### <a name="file-history"></a>文件历史记录  
 文件历史记录是一项功能在 Windows 8.1 和 Windows 8 备份（库、桌面、联系人、收藏夹）的个人资料数据网络共享。 你可以集中管理的文件历史记录设置的所有计算机运行的 Windows 8.1 或 Windows 8 的加入 Windows Server Essentials 网络。 在运行 Windows Server Essentials 服务器上存储的备份数据。 你应该通过中的步骤隐藏客户端还原服务任务[自定义和部署 Windows Server Essentials 在 Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>存储管理  
 存储空间可以聚合物理存储容量的不同的硬盘驱动器、动态添加硬盘驱动器，并创建指定级别的复原的数据量。 在主机上或在虚拟机上，你可以执行此操作。 如果你想要隐藏在运行 Windows Server Essentials 虚拟机此功能，请按[自定义和部署 Windows Server Essentials 在 Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)。  
  
##  <a name="BKMK_Scenarios"></a>测试的方案  
 从托管的角度来讲，我们建议你测试以下情形：  
  

-   [服务器部署](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [服务器配置](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [服务器管理](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [客户端体验](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a>服务器部署  
 你可以在测试服务器以下部署方案：  
  
-   部署 Windows Server 2012 R2 域控制器在实验环境中，运行的服务器，并安装 Windows Server Essentials 体验角色。  
  
-   部署实验环境中运行 Windows Server 2012 R2 的服务器，此服务器加入现有的域中，然后安装了 Windows Server Essentials 体验角色。  
  
-   根据需要自定义 Windows Server Essentials 图像。  
  
-   自动 Windows PowerShell 无人照看的文件与 Windows Server Essentials 部署。  
  
-   迁移到运行 Windows Server Essentials 托管服务器运行 Windows 小型企业服务器本地服务器。  
  
###  <a name="BKMK_ServerConfig2"></a>服务器配置  
 你可以在测试服务器配置以下：  
  
-   配置任意位置的访问权限（虚拟专用网络、远程网站访问和 DirectAccess）。  
  
-   配置存储和 Server 文件夹。  
  
-   打开分支缓存。  
  
-   （如果适用）文件历史记录和配置服务器备份、Online Backup、客户端备份。  
  
-   （如果适用）将配置并管理存储空间。  
  
-   （如果适用）配置（Office 365 和托管的 Exchange Server）的电子邮件解决方案集成。  
  
-   （如果适用）与其他 Microsoft 的联机服务配置集成。  
  
-   （如果适用）配置媒体服务器。  
  
###  <a name="BKMK_ServerManage"></a>服务器管理  
 你可以在测试服务器管理以下：  
  
-   管理用户和组。  
  
-   配置，并且收到的通知的电子邮件通知。  
  
-   运行最佳做法分析工具，以查看是否有错误或条警告消息。  
  
-   System Center Operations Manager 包的配置。  
  
-   如果在操作系统中损坏配置服务器恢复。  
  
###  <a name="BKMK_ClientXP"></a>客户端体验  
 你可以在下面的最终用户方案测试：  
  
-   部署 Internet（PC 或 Mac 的操作系统）的客户端。  
  
-   在客户端上使用启动栏共享文件夹的访问。  
  
-   访问服务器轻远程访问 Web 资产从其他设备（电脑、手机和平板电脑）。  
  
-   Windows Phone 访问我服务器的应用。  
  
-   （如果适用）使用文件历史记录、客户端备份和还原和文件夹重定向。  
  
-   （如果适用）验证电子邮件的集成体验。  
  
##  <a name="BKMK_Support"></a>支持信息  
 你可以下载 Windows Server Essentials 软件开发工具包 (SDK) 和 Windows Server Essentials 评估和部署工具包 (ADK):  
  
-   [Windows Server 软件包软件开发工具包](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [自定义和部署 Windows Server Essentials 在 Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>请参阅  
  
-   [什么是 Windows Server Essentials 中的新增功能](../get-started/what-s-new.md)  

-   [安装 Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [Windows Server Essentials 入门](../get-started/get-started.md)
