---
title: 将 DirectAccess 添加到现有的远程访问 (VPN) 部署
description: 本主题是指南的 Windows Server 2016 的现有远程访问 (VPN) 部署添加 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5db01f7-1ae0-46f2-9be7-8d9e121446b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c6bd11855ec9c5387241ba42babf88c0461a9c4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283584"
---
# <a name="add-directaccess-to-an-existing-remote-access-vpn-deployment"></a>将 DirectAccess 添加到现有的远程访问 (VPN) 部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016
  
## <a name="BKMK_OVER"></a>应用场景说明  
在此方案中，一台运行 Windows Server 2016 中，Windows Server 2012 R2 或 Windows Server 2012 被配置为使用建议的设置的 DirectAccess 服务器已安装并配置 VPN 之后。 如果你想要配置 DirectAccess 的企业功能，例如负载平衡的群集、 多站点部署或双因素客户端身份验证，完成设置单个服务器，本主题中所述的方案，然后设置了企业方案中所述[在企业中部署远程访问](../../ras/Deploy-Remote-Access-in-an-Enterprise.md)。  
  
## <a name="in-this-scenario"></a>本方案内容  
若要设置单个远程访问服务器，需要执行多个规划和部署步骤。  
  
### <a name="planning-steps"></a>规划步骤  
规划分成以下两个阶段：  
  
1.  **规划远程访问基础结构**  
  
    在此阶段，你将描述在开始远程访问部署之前设置网络基础结构所需的规划。 它包括规划网络和服务器拓扑、证书、域名系统 (DNS)、Active Directory 和组策略对象 (GPO) 配置以及 DirectAccess 网络位置服务器。  
  
2.  **规划远程访问部署**  
  
    在此阶段，你将描述准备远程访问部署所需的规划步骤。 它包括规划远程访问客户端计算机、服务器和客户端身份验证要求以及基础结构服务器。  
  
### <a name="deployment-steps"></a>部署步骤  
部署分成以下三个阶段：  
  
1.  **配置远程访问基础结构**  
  
    在此阶段中，你将配置网络和路由、防火墙设置（如有必要）、证书、DNS 服务器、Active Directory 和 GPO 设置以及 DirectAccess 网络位置服务器。  
  
2.  **配置远程访问服务器设置**  
  
    在此阶段，你将配置远程访问客户端计算机、远程访问服务器和基础结构服务器。  
  
3.  **验证部署**  
  
    在此阶段中，你将验证部署是否按要求工作。  
  
## <a name="BKMK_APP"></a>实际应用程序  
部署单一远程访问服务器可提供以下优势：  
  
-   **轻松访问**  
  
    管理运行 Windows 8 和 Windows 7 的计算机可以配置为 DirectAccess 客户端计算机的客户端。 这些客户端只要位于 Internet 上，就可以随时通过 DirectAccess 访问内部网络资源，而无须登录 VPN 连接。 未运行这些操作系统之一的客户端计算机可以通过 VPN 连接到内部网络。 DirectAccess 和 VPN 由同一控制台管理并使用相同的向导集。  
  
-   **易管理性**  
  
    即使客户端计算机不位于企业内部网络，也可以由远程访问管理员通过 DirectAccess 远程管理有权访问 Internet 的 DirectAccess 客户端计算机。 管理服务器可以自动修正不符合公司要求的客户端计算机。  
  
## <a name="BKMK_NEW"></a>角色和此方案所需的功能  
下表列出了本方案所需的角色和功能：  
  
|角色/功能|如何支持本方案|  
|---------|-----------------|  
|远程访问角色|通过使用服务器管理器控制台或 Windows PowerShell 安装或卸载此角色。 本角色包括 DirectAccess（以前是 Windows Server 2008 R2 中的功能）以及路由和远程访问服务（以前是网络策略和访问服务 (NPAS) 服务器角色下的角色服务）。 远程访问角色由以下两个组件组成：<br /><br />1.DirectAccess 以及路由和远程访问服务 (RRAS) VPN：在远程访问管理控制台中管理。<br />2.RRAS 路由：在路由和远程访问控制台中管理。<br /><br />远程访问服务器角色取决于以下服务器功能：<br /><br />Internet 信息服务 (IIS) Web 服务器： 在远程访问服务器和默认 web 探测上配置网络位置服务器所必需的。<br />-Windows 内部数据库：用于远程访问服务器上的本地计帐。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-默认情况下安装远程访问角色时的远程访问服务器上。 支持远程管理控制台用户界面和 Windows PowerShell cmdlet。<br />有选择性地安装在不运行远程访问服务器角色的服务器上。 在此情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />-远程访问 GUI<br />的 Windows PowerShell 远程访问模块<br /><br />依赖项包括：<br /><br />组策略管理控制台<br />-RAS 连接管理器管理工具包 (CMAK)<br />-   Windows PowerShell 3.0<br />图形管理工具和基础结构|  
  
## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  
  
**服务器要求**  
  
-   满足 Windows Server 2012 的硬件要求的计算机。  
  
-   服务器必须至少具有一个已安装、已启用并已加入到内部网络的网络适配器。 当使用两个适配器时，应有一个适配器连接至内部企业网络，另一个连接至外部网络 (Internet)。  
  
-   如果需要 Teredo 作为 IPv4 到 IPv6 转换协议，则服务器的外部适配器需要两个连续的公用 IPv4 地址。 即使存在两个连续的 IP 地址，启用 DirectAccess 向导也不会启用 Teredo。 如果单个 IP 地址可用，那么只有 IP-HTTPS 可用作转换协议。  
  
-   至少一个域控制器。 远程访问服务器和 DirectAccess 客户端都必须是域成员。  
  
-   启用 DirectAccess 向导需要 IP-HTTPS 和网络位置服务器的证书。 如果 SSTP VPN 已使用证书，则该证书会重用于 IP-HTTPS。 如果尚未配置 SSTP VPN，则可配置用于 IP-HTTPS 的证书，或使用自动创建的自签名证书。 对于网络位置服务器，可以配置证书，也可以使用自动创建的自签名证书。  
  
**客户端要求**  
  
-   客户端计算机必须运行 Windows 8 或 Windows 7。  
  
    > [!NOTE]  
    > 仅可将以下操作系统用作 DirectAccess 客户端：Windows Server 2012、 Windows Server 2008 R2、 Windows 8 企业版、 Windows 7 企业版和 Windows 7 旗舰版。  
  
**基础结构和管理服务器要求**  
  
-   在远程管理 DirectAccess 客户端计算机期间，客户端启动与管理服务器的通信，例如域控制器、系统中心配置服务器以及健康注册机构 (HRA) 服务器，这些服务器均可提供 Windows 和防病毒更新以及网络访问保护 (NAP) 客户端兼容性等服务。 开始远程访问部署之前，应该先部署所需的服务器。  
  
-   如果远程访问需要客户端 NAP 兼容性，则应在开始远程访问部署之前部署网络策略服务器 (NPS) 和 HRA。  
  
-   运行 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 SP2 的 DNS 服务器是必需的。  
  
## <a name="BKMK_SOFT"></a>软件要求  
本方案的软件要求包括以下各项：  
  
**服务器要求**  
  
-   远程访问客户端必须使用 V.90 调制解调器。 可以将服务器部署在内部网络边缘，或边缘防火墙或其他设备的后面。  
  
-   如果远程访问服务器位于边缘防火墙或网络地址转换 (NAT) 设备的后面，则该设备必须配置为允许远程访问服务器进出流量。  
  
-   在服务器上部署远程访问的人员需要具有服务器本地管理员权限和域用户权限。 此外，管理员需要有在 DirectAccess 部署中使用的 GPO 的权限。 若要利用将 DirectAccess 部署限制在移动计算机的功能，需要拥有在域控制器上创建 WMI 筛选器的权限。  
  
**远程访问客户端要求**  
  
-   DirectAccess 客户端必须是域成员。 包含客户端的域可属于远程访问服务器所在的同一林中，或者它们可具有与远程访问服务器林或域的双向信任。  
  
-   包含配置为 DirectAccess 客户端的的计算机需要 Active Directory 安全组。 如果配置 DirectAccess 客户端设置时未指定安全组，则在默认情况下，客户端 GPO 将应用于 Domain Computers 安全组中的所有便携式计算机（支持 DirectAccess）。 仅可将以下操作系统用作 DirectAccess 客户端：Windows Server 2012、 Windows Server 2008 R2、 Windows 8 企业版、 Windows 7 企业版和 Windows 7 旗舰版。  
  
    > [!NOTE]  
    > 我们建议你为每个包含将配置为 DirectAccess 客户端的计算机的域创建安全组。  
  

  

