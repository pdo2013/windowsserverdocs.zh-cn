---
title: 步骤2配置高级 DirectAccess 服务器
description: 本主题是 "使用 Windows Server 2016 的高级设置部署单个 DirectAccess 服务器" 指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35afec8e-39a4-463b-839a-3c300ab01174
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0ba2154338871827aae03936e5e39a356a43d675
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388632"
---
# <a name="step-2-configure-advanced-directaccess-servers"></a>步骤2配置高级 DirectAccess 服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍了如何在混合的 IPv4 和 IPv6 环境中，为使用单个远程访问服务器的高级远程访问部署配置其必需的客户端和服务器设置。 在开始执行部署步骤之前，请确保已完成规划[高级 DirectAccess 部署](Plan-an-Advanced-DirectAccess-Deployment.md)中所述的规划步骤。  
  
|任务|描述|  
|----|--------|  
|2.1. 安装远程访问角色|安装远程访问角色。|  
|2.2. 配置部署类型|将部署类型配置为 DirectAccess 和 VPN、仅 DirectAccess 或者仅 VPN。|  
|[规划高级 DirectAccess 部署](Plan-an-Advanced-DirectAccess-Deployment.md)|使用包含 DirectAccess 客户端的安全组配置远程访问服务器。|  
|2.4. 配置远程访问服务器|配置远程访问服务器设置。|  
|2.5. 配置基础结构服务器|配置组织中使用的基础结构服务器。|  
|2.6. 配置应用程序服务器|配置应用程序服务器，以使它们需要身份验证和加密。|  
|2.7. 配置摘要和备用 GPO|查看远程访问配置摘要，并修改 GPO（如果需要）。|  
|2.8. 如何使用 Windows PowerShell 配置远程访问服务器|使用 Windows PowerShell 配置远程访问。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_Role"></a>2.1。 安装远程访问角色  
若要部署远程访问，你必须将远程访问角色安装在组织中即将充当远程访问服务器的服务器上。  
  
#### <a name="to-install-the-remote-access-role"></a>安装远程访问角色  
  
1.  在远程访问服务器上的 "服务器管理器" 控制台的 "**仪表板**" 中，单击 "**添加角色和功能**"。  
  
2.  单击“下一步”三次以打开“选择服务器角色”屏幕。  
  
3.  在“选择服务器角色”页上，选择“远程访问”，单击“添加功能”，然后单击“下一步”。  
  
4.  单击“下一步”五次。  
  
5.  在 **“确认安装选择”** 页上，单击 **“安装”** 。  
  
6.  在“安装进度”页上，确认安装是否已成功，然后单击“关闭”。  
  
@no__t 0Installation 进度成功](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Deploy"></a>2.2。 配置部署类型  
可以通过以下三种方式使用远程访问管理控制台部署远程访问：  
  
-   DirectAccess 和 VPN  
  
-   仅 DirectAccess  
  
-   仅 VPN  
  
本指南中的示例过程使用仅 DirectAccess 的部署。  
  
#### <a name="to-configure-the-deployment-type"></a>配置部署类型  
  
1.  在远程访问服务器上，打开远程访问管理控制台：在 "**开始**" 屏幕上，键入**ramgmtui.exe**，然后按 enter。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
2.  在远程访问管理控制台的中间窗格内，单击“运行远程访问设置向导”。  
  
3.  在“配置远程访问”对话框中，单击部署 DirectAccess 和 VPN、仅 DirectAccess 或者仅 VPN。  
  
## <a name="BKMK_Clients"></a>2.3。 配置 DirectAccess 客户端  
对于要设置为使用 DirectAccess 的客户端计算机，它必须属于所选的安全组。 配置 DirectAccess 后，将安全组中的客户端计算机设置为接收 DirectAccess 组策略对象 (GPO)。 你还可以配置部署方案，它将允许你针对客户端访问和远程管理（或者仅针对远程管理）配置 DirectAccess。  
  
#### <a name="to-configure-directaccess-clients"></a>配置 DirectAccess 客户端  
  
1.  在远程访问管理控制台的中间窗格中，在“步骤 1 远程客户端”区域中单击“配置”。  
  
2.  在 DirectAccess 客户端安装向导的“部署方案”页上，单击你希望在组织中使用的部署方案（“完全 DirectAccess”或“仅远程管理”），然后单击“下一步”。  
  
3.  在“选择组”页上，单击“添加”。  
  
4.  在“选择组”对话框中，选择包含 DirectAccess 客户端计算机的安全组。  
  
    > [!NOTE]  
    > 如果安全组不与远程访问服务器位于同一个林中，则请在完成远程访问设置向导后，单击“任务”窗格中的“刷新管理服务器”，以发现新林中的域控制器和 System Center Configuration Manager 服务器。  
  
5.  如有必要，选中“仅为移动计算机启用 DirectAccess”复选框以仅允许移动计算机访问内部网络。  
  
6.  如有必要，选中“使用强制隧道”复选框，以通过远程访问服务器对所有客户端通信进行路由（路由到内部网络和 Internet）。  
  
7.  单击“下一步”。  
  
8.  在“网络连接助手”页上：  
  
    -   在表中，将用于确定连接性的资源添加到内部网络。 如果未配置任何其他资源，则将自动创建默认 Web 探测。  
  
        > [!CAUTION]  
        > 当你配置用于确定到企业网络的连接性的 Web 探测位置时，请确保你已配置至少一个基于 HTTP 的探测。 仅配置 **ping** 探测是不够的，而且这可能导致无法准确确定连接状态。 这是因为 **ping** 不受 IPsec 保护，因此，它无法确保已正确建立 IPsec 隧道。  
  
    -   添加技术支持电子邮件地址，以允许用户在遇到连接问题时发送信息。  
  
    -   提供 DirectAccess 连接的友好名称。 当用户单击通知区域中的网络图标时，这一名称将显示在网络列表中。  
  
    -   如有必要，选中“允许 DirectAccess 客户端使用本地名称解析”复选框。  
  
        > [!NOTE]  
        > 启用本地名称解析后，运行网络连接助手的用户可以选择使用 DirectAccess 客户端计算机上配置的 DNS 服务器来解析名称。  
  
9. 单击 **“完成”** 。  
  
## <a name="BKMK_Server"></a>2.4。 配置远程访问服务器  
若要部署远程访问，你需要使用以下内容配置远程访问服务器：正确的网络适配器、客户端计算机可可连接到的远程访问服务器的公用 URL（ConnectTo 地址）、具有与 ConnectTo 地址相匹配的使用者的 IP-HTTPS 证书、IPv6 设置，以及客户端计算机身份验证。  
  
#### <a name="to-configure-the-remote-access-server"></a>配置远程访问服务器  
  
1.  在远程访问管理控制台的中间窗格中，在“步骤 2 远程访问服务器”区域中单击“配置”。  
  
2.  在远程访问服务器安装向导的“网络拓扑”页上，单击将在你的组织中使用的部署拓扑。 在“键入客户端用于连接到远程访问服务器的公用名称或 IPv4 地址”中，输入部署的公用名称（此名称与 IP-HTTPS 证书的使用者名称相匹配，例如 edge1.contoso.com），然后单击“下一步”。  
  
3.  在“网络适配器”页上，向导会为你的部署中的网络自动检测网络适配器。 如果向导没有检测到正确的网络适配器，则请手动选择正确的适配器。 该向导还会根据向导上一步中为部署设置的公用名称来自动检测 IP-HTTPS 证书。 如果向导没有检测到正确 IP-HTTPS 证书，请单击“浏览”以手动选择正确的证书，然后单击“下一步”。  
  
4.  在“前缀配置”页上（仅当将 IPv6 部署在内部网络中时，才会出现此页面），向导将自动检测用于内部网络的 IPv6 设置。 如果你的部署需要其他前缀，请配置用于内部网络的 IPv6 前缀、要分配给 DirectAccess 客户端计算机的 IPv6 前缀，以及要分配给 VPN 客户端计算机的 IPv6 前缀。  
  
    > [!NOTE]  
    > 你可以使用分号分隔的列表指定多个内部 IPv6 前缀，例如，2001:db8:1::/48;2001:db8:2::/48。  
  
5.  在“身份验证”页上：  
  
    -   在“用户身份验证”中单击“Active Directory 凭据”。 若要使用双重身份验证配置部署，请单击“双重身份验证”。 有关详细信息，请参阅[使用 OTP 身份验证部署远程访问](https://technet.microsoft.com/library/hh831379.aspx)。  
  
    -   对于多站点和双重身份验证部署，必须使用计算机证书身份验证。 选中“使用计算机证书”复选框以使用计算机证书身份验证，然后选择 IPsec 根证书。  
  
    -   要使 Windows 7 客户端计算机能够通过 DirectAccess 进行连接，请选中 "**使 windows 7 客户端计算机能够通过 directaccess 进行连接**" 复选框。  
  
        > [!NOTE]  
        > 在这种类型的部署中，还必须使用计算机证书身份验证。  
  
6.  单击 **“完成”** 。  
  
## <a name="BKMK_Infra"></a>2.5。 配置基础结构服务器  
若要在远程访问部署中配置基础结构服务器，你必须配置网络位置服务器、DNS 设置（包括 DNS 后缀搜索列表），以及远程访问未自动检测到的管理服务器。  
  
#### <a name="to-configure-the-infrastructure-servers"></a>配置基础结构服务器  
  
1.  在远程访问管理控制台的中间窗格中，在“步骤 3 基础结构服务器”区域中单击“配置”。  
  
2.  在基础结构服务器设置向导的“网络位置服务器”页上，单击与你部署中网络位置服务器的位置相对应的选项。 如果网络位置服务器位于远程 Web 服务器上，则请在继续之前输入 URL 并单击“验证”。 如果网络位置服务器位于远程访问服务器上，则单击“浏览”以查找相关的证书，然后单击“下一步”。  
  
3.  在“DNS”页上的表中，输入将应用为名称解析策略表 (NRPT) 免除的任何其他名称后缀。 选择本地名称解析选项，然后单击“下一步”。  
  
4.  在“DNS 后缀搜索列表”页上，远程访问服务器会自动检测部署中任何的域后缀。 使用“添加”和“删除”按钮从要使用的域后缀列表中添加和删除域后缀。 若要添加新的域后缀，请在“新后缀”中输入该后缀，然后单击“添加”。 单击“下一步”。  
  
5.  在“管理”页上，添加任何未自动检测到的管理服务器，然后单击“下一步”。 远程访问将自动添加域控制器和 System Center Configuration Manager 服务器。  
  
    > [!NOTE]  
    > 虽然自动添加服务器，但不会显示在列表中。 在你第一次应用配置后，System Center Configuration Manager 服务器将显示在列表中。  
  
6.  单击 **“完成”** 。  
  
## <a name="BKMK_App"></a>2.6。 配置应用程序服务器  
在远程访问部署中，配置应用程序服务器是可选任务。 远程访问使你能够要求对所选应用程序服务器进行身份验证，这由它们在应用程序服务器安全组中的包含情况确定。 默认情况下，还会对要求身份验证的应用程序服务器的通信进行加密；但是，你可以选择不加密应用程序服务器的通信且仅使用身份验证。  
  
> [!NOTE]  
> 仅在运行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的应用程序服务器上支持不加密的身份验证。  
  
#### <a name="to-configure-application-servers"></a>配置应用程序服务器  
  
1.  在远程访问管理控制台的中间窗格中，在“步骤 4 应用程序服务器”区域中单击“配置”。  
  
2.  在 DirectAccess 应用程序服务器安装向导中，如果需要对所选应用程序服务器进行身份验证，请单击“将身份验证扩展到所选择的应用程序服务器”。 单击“添加”以选择应用程序服务器安全组。  
  
3.  若要将访问权限仅限制在应用程序服务器安全组中的服务器，请选中“只允许访问安全组中包含的服务器”复选框。  
  
4.  若要在不加密的情况下使用身份验证，请选择 "不加密流量 @no__t 0Do"。仅使用 "仅身份验证 @ no__t" 复选框。  
  
5.  单击 **“完成”** 。  
  
## <a name="BKMK_GPO"></a>2.7。 配置摘要和备用 GPO  
远程访问配置完成后，将显示“远程访问审阅”。 你可以审阅之前选择的所有设置，包括：  
  
1.   “GPO 设置”：将列出 DirectAccess 服务器 GPO 名称和客户端 GPO 名称。 此外，你可以单击“GPO 设置”标题旁边的“更改”链接以修改 GPO 设置。  
  
2.   “远程客户端”：将显示 DirectAccess 客户端配置，包括安全组、强制隧道状态、连接性验证程序和 DirectAccess 连接名称。  
  
3.   “远程访问服务器”：将显示 DirectAccess 配置，包括公用名称/地址、网络适配器配置、证书信息和 OTP 信息（若已配置）。  
  
4.   “基础结构服务器”：此列表包括网络位置服务器 URL、DirectAccess 客户端使用的 DNS 后缀和管理服务器信息。  
  
5.   “应用程序服务器”：除了显示特定应用程序服务器的端到端身份验证的状态以外，还将显示 DirectAccess 远程管理状态。  
  
## <a name="BKMK_PS"></a>2.8。 如何使用 Windows PowerShell 配置远程访问服务器  
@no__t 0Windows PowerShell](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)**Windows powershell 等效命令**  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要通过使用以下参数，仅在带有根 **corp.contoso.com** 的域中在远程访问的边缘拓扑中为 DirectAccess 执行完全安装：服务器 GPO：**DirectAccess Server Settings**，客户端 GPO：DirectAccess Client Settings，内部网络适配器：**Corpnet**，外部网络适配器：**Internet**、ConnectTto address： **edge1.contoso.com**和网络位置服务器： **nls.corp.contoso.com**：  
  
```  
Install-RemoteAccess -Force -PassThru -ServerGpoName 'corp.contoso.com\DirectAccess Server Settings' -ClientGpoName 'corp.contoso.com\DirectAccess Client Settings' -DAInstallType 'FullInstall' -InternetInterface 'Internet' -InternalInterface 'Corpnet' -ConnectToAddress 'edge1.contoso.com' -NlsUrl 'https://nls.corp.contoso.com/'  
```  
  
若要将远程访问服务器配置为使用计算机证书身份验证，并具有由名为 CORP-APP1-CA 的证书颁发机构颁发的 IPsec 根证书：  
  
```  
$certs = Get-ChildItem Cert:\LocalMachine\Root  
$IPsecRootCert = $certs | Where-Object {$_.Subject -Match "corp-APP1-CA"}  
Set-DAServer -IPsecRootCertificate $IPsecRootCert  
```  
  
若要添加包含名为 **DirectAccessClients** 的 DirectAccess 客户端的安全组，并删除默认域计算机安全组：  
  
```  
Add-DAClient -SecurityGroupNameList @('corp.contoso.com\DirectAccessClients')  
Remove-DAClient -SecurityGroupNameList @('corp.contoso.com\Domain Computers')  
```  
  
为所有计算机（不仅限于笔记本和便携式计算机）启用远程访问，并为 Windows 7 客户端启用远程访问：  
  
```  
Set-DAClient -OnlyRemoteComputers 'Disabled' -Downlevel 'Enabled'  
```  
  
若要配置 DirectAccess 客户端体验，包括友好连接名称和 Web 探测 URL：  
  
```  
Set-DAClientExperienceConfiguration -FriendlyName 'Contoso DirectAccess Connection' -PreferLocalNamesAllowed $False -PolicyStore 'corp.contoso.com\DirectAccess Client Settings' -CorporateResources @('HTTP:https://directaccess-WebProbeHost.corp.contoso.com')  
```  
  
## <a name="BKMK_Links"></a>上一步  
  
-   [步骤 1：配置高级 DirectAccess 基础结构](da-adv-configure-s1-infrastructure.md)  
  
## <a name="next-step"></a>下一步  
  
-   [步骤 3：验证部署](Step-3-Verify-the-Deployment.md)  
  


