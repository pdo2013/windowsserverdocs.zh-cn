---
title: 使用高级设置部署单个 DirectAccess 服务器
description: 本主题是指南部署单个 DirectAccess 服务器使用高级设置的 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b211a9ca-1208-4e1f-a0fe-26a610936c30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f17e1c9dd1a4e2d064a4e5980904c524dc62fb72
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283603"
---
# <a name="deploy-a-single-directaccess-server-with-advanced-settings"></a>使用高级设置部署单个 DirectAccess 服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍的 DirectAccess 方案使用单个 DirectAccess 服务器，并允许你使用高级设置部署 DirectAccess。  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>在开始部署之前，请参阅不受支持的配置、已知问题和先决条件的列表  
可以使用以下主题以查看先决条件和其他信息，然后再部署 DirectAccess。  
  
-   [DirectAccess 不受支持的配置](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [部署 DirectAccess 的先决条件](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>应用场景说明  
在此方案中，运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 中，一台计算机配置为 DirectAccess 服务器使用高级设置。  
  
> [!NOTE]  
> 如果你想要使用简单的设置配置基本部署，请参阅[部署单个 DirectAccess 服务器使用开始向导](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)。 在简单的方案中，将通过向导使用默认设置配置 DirectAccess，而无需配置基础结构设置，例如，证书颁发机构 (CA) 或 Active Directory 安全组。  
  
## <a name="in-this-scenario"></a>本方案内容  
若要使用高级设置来设置单个 DirectAccess 服务器，必须完成几个规划和部署步骤。  
  
### <a name="prerequisites"></a>先决条件  
在开始之前，你可以查看以下要求。  
  
-   必须在所有配置文件上启用 Windows 防火墙。  
  
-   DirectAccess 服务器是网络位置服务器。  
  
-   你希望在其中安装 DirectAccess 服务器的域中的所有无线计算机都已启用 DirectAccess。 当你部署 DirectAccess 时，它会在当前域中的所有移动计算机上自动启用。  
  
> [!IMPORTANT]  
> 部署 DirectAccess 时，不支持某些技术和配置。  
>   
> -   不支持企业网络中的站点内自动隧道寻址协议 (ISATAP)。 如果你正在使用 ISATAP，应将其删除并使用本机 IPv6。  
  
### <a name="planning-steps"></a>规划步骤  
规划分成以下两个阶段：  
  
1.  **规划 DirectAccess 基础结构**。 本阶段描述在开始 DirectAccess 部署之前设置网络基础结构所需的规划。 它包括规划网络和服务器拓扑、计划证书、DNS、Active Directory 和组策略对象 (GPO) 配置以及 DirectAccess 网络位置服务器。  
  
2.  **规划 DirectAccess 部署**。 本阶段描述准备 DirectAccess 部署所需的规划步骤。 它包括规划 DirectAccess 客户端计算机、服务器和客户端身份验证要求、VPN 设置、基础设施服务器以及管理和应用程序服务器。  
  
### <a name="deployment-steps"></a>部署步骤  
部署分成以下三个阶段：  
  
1.  **配置 DirectAccess 基础结构**。 本阶段包括配置网络和路由、配置防火墙设置（如有必要）、配置证书、DNS 服务器、Active Directory 和 GPO 设置以及 DirectAccess 网络位置服务器。  
  
2.  **配置 DirectAccess 服务器设置**。 本阶段包括用于配置 DirectAccess 客户端计算机、DirectAccess 服务器、基础结构服务器、管理和应用程序服务器的步骤。  
  
3.  **验证部署**。 本阶段包括用于验证 DirectAccess 部署的步骤。  
  
有关详细的部署步骤，请参阅 [Install and Configure Advanced DirectAccess](../../../remote-access/directaccess/single-server-advanced/Install-and-Configure-Advanced-DirectAccess.md)。  
  
## <a name="BKMK_APP"></a>实际应用程序  
部署单个 DirectAccess 服务器可提供以下功能：  
  
-   **轻松访问**。 运行 Windows 10、 Windows 8.1、 Windows 8 和 Windows 7 的托管客户端计算机可以配置为 DirectAccess 客户端计算机。 这些客户端只要位于 Internet 上，就可以随时通过 DirectAccess 访问内部网络资源，无须登录 VPN 连接。 未运行这些操作系统之一的客户端计算机可以通过 VPN 连接到内部网络。  
  
-   **易于管理**。 远程访问管理员可以通过 DirectAccess 远程管理 Internet 上的 DirectAccess 客户端计算机，即使客户端计算机不在企业内部网络中也是如此。 管理服务器可以自动修正不符合公司要求的客户端计算机。 DirectAccess 和 VPN 由同一控制台管理并具有相同的向导集。 此外，可从单个远程访问管理控制台管理一台或多台 DirectAccess 服务器  
  
## <a name="BKMK_NEW"></a>角色和此方案所需的功能  
下表列出了本方案所需的角色和功能：  
  
|角色/功能|如何支持本方案|  
|---------|-----------------|  
|远程访问角色|使用服务器管理器控制台或 Windows PowerShell 安装或卸载此角色。 本角色包括 DirectAccess 和路由以及远程访问服务 (RRAS)。 远程访问角色由以下两个组件组成：<br/><br/>1.DirectAccess 和 RRAS VPN。 DirectAccess 和 VPN 远程访问管理控制台中一起管理。<br/>2.RRAS 路由。 在传统路由和远程访问控制台中管理 RRAS 路由功能。<br /><br />远程访问服务器角色取决于以下服务器角色/功能：<br/><br/> Internet 信息服务 (IIS) Web 服务器-此功能是需要在 DirectAccess 服务器和默认 web 探测上配置网络位置服务器。<br/> -Windows 内部数据库。 用于 DirectAccess 服务器上的本地帐户。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-如果远程访问角色安装，并且支持远程管理控制台用户界面和 Windows PowerShell cmdlet 它是默认情况下，DirectAccess 服务器上安装。<br />-它可以在不运行 DirectAccess 服务器角色的服务器上选择性地安装。 在这种情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />-远程访问图形用户界面 (GUI)<br />的 Windows PowerShell 远程访问模块<br /><br />依赖项包括：<br /><br />组策略管理控制台<br />-RAS 连接管理器管理工具包 (CMAK)<br />-   Windows PowerShell 3.0<br />图形管理工具和基础结构|  
  
## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  
  
-   服务器要求：  
  
    -   满足 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的硬件要求的计算机。  
  
    -   服务器必须至少安装和启用了一个网络适配器，并连接至内部网络。 当使用两个适配器时，应有一个适配器连接至内部企业网络，另一个连接至外部网络（Internet 或专用网络）。  
  
    -   如果需要 Teredo 作为 IPv4 到 IPv6 转换协议，则服务器的外部适配器需要两个连续的公用 IPv4 地址。 如果单个 IP 地址可用，那么仅 IP-HTTPS 可以用作转换协议。  
  
    -   至少一个域控制器。 DirectAccess 服务器和 DirectAccess 客户端都必须是域成员。  
  
    -   如果你不希望为 IP-HTTPS 或网络位置服务器使用自签名证书，或者如果你希望使用客户端 IPsec 身份验证的客户端证书，则需要证书颁发机构 (CA)。 或者，你可以从公共 CA 申请证书。  
  
    -   如果网络位置服务器未位于 DirectAccess 服务器上，则需要单独的 Web 服务器来运行它。  
  
-   客户端的要求：  
  
    -   客户端计算机必须运行 Windows 10、 Windows 8 或 Windows 7。  
  
        > [!NOTE]  
        > 在以下操作系统用作 DirectAccess 客户端：Windows 10、 Windows Server 2012 R2、 Windows Server 2012 中，Windows 8 企业版、 Windows 7 企业版或 Windows 7 旗舰版。  
  
-   基础机构和管理服务器要求：  
  
    -   在远程管理 DirectAccess 客户端计算机期间，客户端启动与管理服务器的通信，例如域控制器、System Center Configuration Server 以及健康注册机构 (HRA) 服务器，这些服务器均可提供 Windows 和防病毒更新以及网络访问保护 (NAP) 客户端兼容性等服务。 开始远程访问部署之前，先部署所需的服务器。  
  
    -   如果远程访问需要客户端 NAP 合规性，则在开始远程访问部署之前，应该先部署 NPS 和 HRS 服务器  
  
    -   如果启用了 VPN，则在不使用静态地址池的情况下，需要使用 DHCP 服务器，以将 IP 地址自动分配给 VPN 客户端。  
  
## <a name="BKMK_SOFT"></a>软件要求  
存在许多对本方案的要求。  
  
-   服务器要求：  
  
    -   DirectAccess 服务器必须是域成员。 可以将服务器部署在内部网络边缘，或边缘防火墙或其他设备的后面。  
  
    -   如果 DirectAccess 服务器位于边缘防火墙或 NAT 设备的后面，则该设备必须配置为允许 DirectAccess 服务器进出流量。  
  
    -   在服务器上部署远程访问需要拥有服务器本地管理员权限和域用户权限。 此外，管理员需要具备在 DirectAccess 部署中使用 GPO 的权限。 若要利用将 DirectAccess 部署局限在移动计算机的功能，需要在域控制器上创建 WMI 筛选器的权限。  
  
-   远程访问客户端要求：  
  
    -   DirectAccess 客户端必须是域成员。 含有客户端的域可属于与 DirectAccess 服务器相同的林，或者拥有 DirectAccess 服务器林或域的双向信任。  
  
    -   包含配置为 DirectAccess 客户端的的计算机需要 Active Directory 安全组。 如果配置 DirectAccess 客户端设置时未指定安全组，客户端 GPO 默认应用于 Domain Computers 安全组中的所有便携式计算机。  
  
        > [!NOTE]  
        > 建议你为包含 DirectAccess 客户端计算机的每个域创建安全组。  
  
        > [!IMPORTANT]  
        > 如果在 DirectAccess 部署中，启用了 Teredo，并且你想要提供对 Windows 7 客户端访问权限，请确保客户端将升级到 Windows 7 SP1。 使用 Windows 7 RTM 的客户端将无法再通过 Teredo 进行连接。 但是，这些客户端仍将能够通过 IP-HTTPS 连接到企业网络。  
  
## <a name="BKMK_LINKS"></a>另请参阅  
下表提供其他资源的链接。  
  
|内容类型|参考|  
|--------|-------|  
|**部署**|[Windows Server 中的 DirectAccess 部署路径](../../../remote-access/directaccess/DirectAccess-Deployment-Paths-in-Windows-Server.md)<br /><br />[部署单个 DirectAccess 服务器使用开始的向导](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|  
|**工具和设置**|[远程访问 PowerShell cmdlet](https://technet.microsoft.com/library/hh918399.aspx)|  
|**社区资源**|[DirectAccess 生存指南](https://social.technet.microsoft.com/wiki/contents/articles/23210.directaccess-survival-guide.aspx)<br /><br />[DirectAccess Wiki 条目](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**相关技术**|[IPv6 的工作原理](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


