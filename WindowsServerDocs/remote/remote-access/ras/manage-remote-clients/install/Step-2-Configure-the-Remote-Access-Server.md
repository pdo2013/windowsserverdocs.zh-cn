---
title: 步骤 2 配置远程访问服务器
description: 本主题是指南的在 Windows Server 2016 中远程管理 DirectAccess 客户端的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a99a2667733c1a23bcba1134d59223044dc000b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845528"
---
# <a name="step-2-configure-the-remote-access-server"></a>步骤 2 配置远程访问服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍如何配置所需的远程管理 DirectAccess 客户端的客户端和服务器设置。 开始执行部署步骤之前，请确保已完成中所述的规划步骤[步骤 2 规划远程访问部署](../plan/Step-2-Plan-the-Remote-Access-Deployment.md)。  
  
|任务|描述|  
|----|--------|  
|安装远程访问角色|安装远程访问角色。|  
|配置部署类型|将部署类型配置为 DirectAccess 和 VPN、仅 DirectAccess 或者仅 VPN。|  
|配置 DirectAccess 客户端|使用包含 DirectAccess 客户端的安全组配置远程访问服务器。|  
|配置远程访问服务器|配置远程访问服务器设置。|  
|配置基础结构服务器|配置组织中使用的基础结构服务器。|  
|配置应用程序服务器|配置应用程序服务器需要身份验证和加密。|  
|配置摘要和备用 GPO|查看远程访问配置摘要，并修改 GPO（如果需要）。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_Role"></a>安装远程访问角色  
将充当远程访问服务器在组织中，必须在服务器上安装远程访问角色。  
  
#### <a name="to-install-the-remote-access-role"></a>安装远程访问角色  
  
### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>若要在 DirectAccess 服务器上安装远程访问角色  
  
1.  在 DirectAccess 服务器上，在服务器管理器控制台中，在**仪表板**，单击**添加角色和功能**。  
  
2.  单击 **“下一步”** 三次以打开服务器角色选择屏幕。  
  
3.  上**选择服务器角色**对话框中，选择**远程访问**，然后单击**下一步**。  
  
4.  单击**下一步**三次。  
  
5.  上**选择角色服务**对话框中，选择**DirectAccess 和 VPN (RAS)** ，然后单击**添加功能**。  
  
6.  选择**路由**，选择**Web 应用程序代理**，单击**添加功能**，然后单击**下一步**。  
  
7. 单击 **“下一步”**，然后单击 **“安装”**。  
  
8.  在“安装进度”对话框中，验证安装是否成功，然后单击“关闭”。  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Deploy"></a>配置部署类型  
有三个选项，可用于从远程访问管理控制台部署远程访问：  
  
-   DirectAccess 和 VPN  
  
-   仅 DirectAccess  
  
-   仅 VPN  
  
> [!NOTE]  
> 本指南中的示例过程使用 DirectAccess 的部署的唯一方法。  
  
#### <a name="to-configure-the-deployment-type"></a>配置部署类型  
  
1.  在远程访问服务器上，打开远程访问管理控制台：上**启动**屏幕、 类型、 类型**远程访问管理控制台**，然后按 ENTER。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
2.  在远程访问管理控制台的中间窗格内，单击“运行远程访问设置向导”。  
  
3.  在中**配置的远程访问**对话框中，选择 DirectAccess 和 VPN、 仅 DirectAccess 或仅 VPN。  
  
## <a name="BKMK_Clients"></a>配置 DirectAccess 客户端  
对于要设置为使用 DirectAccess 的客户端计算机，它必须属于所选的安全组。 配置 DirectAccess 后，安全组中的客户端计算机都设置为接收远程管理 DirectAccess 组策略对象 (Gpo)。  
  
#### <a name="to-configure-directaccess-clients"></a>配置 DirectAccess 客户端  
  
1.  在远程访问管理控制台的中间窗格中，在“步骤 1 远程客户端”区域中单击“配置”。  
  
2.  在 DirectAccess 客户端安装程序向导中，在**部署方案**页上，单击**仅针对远程管理部署 DirectAccess**，然后单击**下一步**。  
  
3.  在“选择组”页上，单击“添加”。  
  
4.  在中**选择组**对话框框中，选择安全组，包含 DirectAccess 客户端计算机，然后单击**下一步**。  
  
5.  在“网络连接助手”页上：  
  
    -   在表中，添加将用于确定连接到内部网络的资源。 如果未配置任何其他资源，则将自动创建默认 Web 探测。 在配置用于确定到企业网络的连接的 web 探测位置时，请确保你已配置的至少一个基于 HTTP 探测。 配置仅 ping 探测是不够的并可能导致不准确确定连接状态。 这是因为从 IPsec 免除 ping。 因此，ping 不能确保已正确建立 IPsec 隧道。  
  
    -   添加技术支持电子邮件地址，以允许用户在遇到连接问题时发送信息。  
  
    -   提供 DirectAccess 连接的友好名称。  
  
    -   如有必要，选中“允许 DirectAccess 客户端使用本地名称解析”复选框。  
  
        > [!NOTE]  
        > 启用本地名称解析后，运行 NCA 的用户可以通过使用 DirectAccess 客户端计算机配置的 DNS 服务器解析名称。  
  
6.  单击 **“完成”**。  
  
## <a name="BKMK_Server"></a>配置远程访问服务器  
若要部署远程访问，需要配置将充当远程访问服务器提供以下服务器：  
  
1.  正确的网络适配器  
  
2.  远程访问服务器向哪些客户端计算机可以连接 （ConnectTo 地址） 的公共 URL  
  
3.  带有与 ConnectTo 地址相匹配的使用者的 IP-HTTPS 证书  
  
4.  IPv6 设置  
  
5.  客户端计算机身份验证  
  
#### <a name="to-configure-the-remote-access-server"></a>配置远程访问服务器  
  
1.  在远程访问管理控制台的中间窗格中，在“步骤 2 远程访问服务器”区域中单击“配置”。  
  
2.  在远程访问服务器安装向导的“网络拓扑”页上，单击将在你的组织中使用的部署拓扑。 在“键入客户端用于连接到远程访问服务器的公用名称或 IPv4 地址”中，输入部署的公用名称（此名称与 IP-HTTPS 证书的使用者名称相匹配，例如 edge1.contoso.com），然后单击“下一步”。  
  
3.  上**网络适配器**页上，向导会自动检测：  
  
    -   在部署中的网络的网络适配器。 如果向导没有检测到正确的网络适配器，则请手动选择正确的适配器。  
  
    -   IP-HTTPS 证书。 这基于向导的上一步骤中设置的部署的公共名称。 如果向导没有检测到正确 IP-HTTPS 证书，单击**浏览**手动选择正确的证书。  
  
4.  单击“下一步” 。  
  
5.  上**前缀配置**（此页面时才会显示 IPv6 检测到内部网络中） 的页上，向导会自动检测到内部网络使用的 IPv6 设置。 如果你的部署需要其他前缀，请配置用于内部网络的 IPv6 前缀、要分配给 DirectAccess 客户端计算机的 IPv6 前缀，以及要分配给 VPN 客户端计算机的 IPv6 前缀。  
  
6.  在“身份验证”页上：  
  
    -   对于多站点和双重身份验证部署，必须使用计算机证书身份验证。 选择**使用计算机证书**复选框以使用计算机证书身份验证，然后选择 IPsec 根证书。  
  
    -   若要启用客户端计算机运行 Windows 7 通过 DirectAccess 进行连接，请选择**使 Windows 7 客户端计算机能够通过 DirectAccess 进行连接**复选框。 在这种类型的部署中，还必须使用计算机证书身份验证。  
  
7.  单击 **“完成”**。  
  
## <a name="BKMK_Infra"></a>配置基础结构服务器  
若要在远程访问部署中配置的基础结构服务器，必须进行下列配置：  
  
-   网络位置服务器  
  
-   DNS 设置，包括 DNS 后缀搜索列表  
  
-   不会自动检测远程访问任何管理服务器  
  
#### <a name="to-configure-the-infrastructure-servers"></a>配置基础结构服务器  
  
1.  在远程访问管理控制台的中间窗格中，在“步骤 3 基础结构服务器”区域中单击“配置”。  
  
2.  在基础结构服务器设置向导的“网络位置服务器”页上，单击与你部署中网络位置服务器的位置相对应的选项。  
  
    -   如果网络位置服务器位于远程 web 服务器上，输入 URL，然后依次**验证**，并继续。  
  
    -   如果网络位置服务器位于远程访问服务器上，则单击“浏览”以查找相关的证书，然后单击“下一步”。  
  
3.  上**DNS**页上，在表中，输入其他名称后缀，将应用为名称解析策略表 (NRPT) 免除。 选择本地名称解析选项，然后单击“下一步”。  
  
4.  上**DNS 后缀搜索列表**页上，远程访问服务器自动检测部署中的域后缀。 使用**外**并**删除**按钮，以创建你想要使用的域后缀的列表。 若要添加新的域后缀，请在“新后缀”中输入该后缀，然后单击“添加”。 单击“下一步” 。  
  
5.  上**管理**页上，将不会自动检测到的管理服务器添加，然后单击**下一步**。 远程访问将自动添加域控制器和 System Center Configuration Manager 服务器。  
  
6.  单击 **“完成”**。  
  
## <a name="BKMK_App"></a>配置应用程序服务器  
在完整的远程访问部署中，配置应用程序服务器是可选的任务。 在此方案中的远程管理 DirectAccess 客户端，都不能使用应用程序服务器和此步骤中灰显，以指示未处于活动状态。 单击**完成**以应用配置。  
  
## <a name="BKMK_GPO"></a>配置摘要和备用 Gpo  
远程访问配置完成后，将显示“远程访问审阅”。 你可以审阅之前选择的所有设置，包括：  
  
-   **GPO 设置**  
  
    将列出 DirectAccess 服务器 GPO 名称和客户端 GPO 名称。 可以单击**更改**链接旁边**GPO 设置**标题以修改 GPO 设置。  
  
-   **远程客户端**  
  
    将显示 DirectAccess 客户端配置，包括安全组、 连接性验证程序和 DirectAccess 连接名称。  
  
-   **远程访问服务器**  
  
    将显示 DirectAccess 配置，包括公用名称和地址、 网络适配器配置和证书信息。  
  
-   **基础结构服务器**  
  
    此列表包括网络位置服务器 URL、DirectAccess 客户端使用的 DNS 后缀和管理服务器信息。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 3：验证部署](Step-3-Verify-the-Deployment_2.md)  
  
  


