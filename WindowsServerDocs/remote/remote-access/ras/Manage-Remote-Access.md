---
title: 管理远程访问
description: 本主题提供有关如何管理 Windows Server 2016 中的远程访问的信息。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1459819a-b1b6-4800-8770-4a85d02c7a2b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b992f302378c103b242537c97e5d4b41e382b9cd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876228"
---
# <a name="manage-remote-access"></a>管理远程访问

>适用于：Windows 服务器 （半年频道），Windows Server 2016

DirectAccess 远程客户端管理部署方案使用 DirectAccess 通过 Internet 维护客户端。 本部分介绍了该方案，包括其阶段、角色、功能以及指向其他资源的链接。  
  
Windows Server 2016 和 Windows Server 2012 将 DirectAccess 以及路由和远程访问服务 (RRAS) VPN 合并到单个远程访问角色。   
  
> [!NOTE]  
> 除本主题外，还提供了以下远程访问管理主题。  
>   
> -   [使用远程访问监视和记帐](monitoring-and-accounting/Use-Remote-Access-Monitoring-and-Accounting.md)  
> -   [远程管理 DirectAccess 客户端](manage-remote-clients/Manage-DirectAccess-Clients-Remotely.md)  
  
## <a name="BKMK_OVER"></a>应用场景说明  
DirectAccess 客户端计算机连接到 Internet 的同时也会连接到 Intranet（无论用户是否已登录该计算机）。 可以将它们当作 Intranet 资源进行管理，并与组策略更改、操作系统更新、反恶意软件更新和其他组织更改保持同步。  
  
在某些情形中，Intranet 服务器或计算机必须启动与 DirectAccess 客户端的连接。 例如，技术支持部门的技术人员可以使用远程桌面连接连接远程 DirectAccess 客户端并进行故障排除。 在使用 DirectAccess 进行远程管理时，本方案可让你保留用户连接的现有远程访问解决方案。  
  
DirectAccess 提供支持远程管理 DirectAccess 客户端的配置。 可以使用部署向导选项限制仅可针对那些需要远程管理客户端计算机的情况创建策略。  
  
> [!NOTE]  
> 在此部署中，不可使用用户级配置选项，比如强制隧道、网络访问保护 (NAP) 集成以及双因素身份验证。  
  
## <a name="in-this-scenario"></a>本方案内容  
DirectAccess 远程客户端管理部署方案包括以下规划和配置步骤。  
  
### <a name="plan-the-deployment"></a>规划部署  
规划此方案仅有几项计算机和网络要求。 其中包括：  
  
-   **网络和服务器拓扑**：你可以使用 DirectAccess 将远程访问服务器放置在 Intranet 的边缘或者网络地址转换 (NAT) 设备或防火墙后面。  
  
-   **DirectAccess 网络位置服务器**：DirectAccess 客户端使用网络位置服务器来确定它们是否位于内部网络上。 网络位置服务器可安装在 DirectAccess 服务器或其他服务器上。  
  
-   **DirectAccess 客户端**：确定将配置为 DirectAccess 客户端的托管服务器。  
  
### <a name="configure-the-deployment"></a>配置部署  
配置部署包含多个步骤。 这些问题包括：  
  
1.  **配置基础结构**：配置 DNS 设置，将服务器和客户端计算机加入域（如有必要），并配置 Active Directory 安全组。  
  
    在此部署方案中，远程访问将自动创建组策略对象 (GPO)。 有关高级的证书 GPO 选项，请参阅[部署高级远程访问](assetId:///3475e527-541f-4a34-b940-18d481ac59f6)。  
  
2.  **配置远程访问服务器和网络设置**：配置网络适配器、IP 地址和路由。  
  
3.  **配置证书设置**：在此部署方案中，入门向导将创建自签名的证书，因此无需配置更高级的证书基础结构。  
  
4.  **配置网络位置服务器**：在此方案中，网络位置服务器将安装在远程访问服务器上。  
  
5.  **规划 DirectAccess 管理服务器**：管理员可以使用 Internet 远程管理位于企业网络之外的 DirectAccess 客户端计算机。 管理服务器包括在远程客户端管理过程中使用的计算机（如更新服务器）。  
  
6.  **配置远程访问服务器**：安装远程访问角色并运行 DirectAccess 开始向导来配置 DirectAccess。  
  
7.  **验证部署**：测试客户端，以便确保其能使用 DirectAccess 连接到内部网络和 Internet。  
  
## <a name="BKMK_APP"></a>实际应用程序  
部署单一远程访问服务器管理 DirectAccess 客户端可提供以下优势：  
  
-   **轻松访问**：管理运行 Windows 8 或 Windows 7 的计算机可以配置为 DirectAccess 客户端计算机的客户端。 这些客户端只要连接到 Internet，就可以随时通过 DirectAccess 访问内部网络资源，而无需登录 VPN 连接。 未运行这些操作系统之一的客户端计算机可以通过 VPN 连接到内部网络。 DirectAccess 和 VPN 由同一控制台管理并使用相同的向导集。  
  
-   **易于管理**：远程访问管理员可以使用 DirectAccess 远程管理连接到 Internet 的 DirectAccess 客户端计算机，即使客户端计算机不在企业内部网络中也可以。 管理服务器可以自动修正不符合公司要求的客户端计算机。 单个远程访问管理控制台可以管理一个或多个远程访问服务器。  
  
## <a name="BKMK_NEW"></a>在此方案中包括角色和功能  
下表列出了本方案所需的角色和功能：  
  
|角色或功能|如何支持本方案|  
|----------|-----------------|  
|*远程访问角色*|使用服务器管理器控制台或 Windows PowerShell 安装和卸载此角色。 本角色包括 DirectAccess（以前是 Windows Server 2008 R2 中的功能）以及路由和远程访问服务（以前是网络策略和访问服务 (NPAS) 服务器角色下的角色服务）。 远程访问角色由以下两个组件组成：<br /><br />1.DirectAccess 以及路由和远程访问服务 (RRAS) VPN：在远程访问管理控制台中管理 DirectAccess 和 VPN。<br />2.RRAS：在路由和远程访问控制台中管理功能。<br /><br />远程访问服务器角色依赖以下功能：<br /><br />-Web 服务器 (IIS):用于配置网络位置服务器和默认 Web 探测。<br />-Windows 内部数据库：用于远程访问服务器上的本地计帐。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-默认情况下如果远程访问角色安装并且支持远程管理控制台用户界面的远程访问服务器上。<br />-为不运行远程访问服务器角色的服务器上的选项。 在此情况下，它可用于远程管理远程访问服务器。<br /><br />此功能由下列组件构成：<br /><br />-远程访问 GUI 和命令行工具<br />的 Windows PowerShell 远程访问模块<br /><br />依赖项包括：<br /><br />组策略管理控制台<br />-RAS 连接管理器管理工具包 (CMAK)<br />-   Windows PowerShell 3.0<br />图形管理工具和基础结构|  
  
## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  
  
### <a name="server-requirements"></a>服务器要求  
  
-   满足 Windows Server 2016 的硬件要求的计算机。 有关详细信息，请参阅 Windows Server 2016[系统要求](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements-and-installation)。  
  
-   服务器必须至少安装和启用了一个网络适配器。 应该只有一个适配器连接到企业内部网络，也只有一个适配器连接到外部网络 (Internet)。  
  
-   如果需要 Teredo 作为 IPv6 到 IPv4 转换协议，则服务器的外部适配器需要两个连续的公用 IPv4 地址 如果只有一个网络适配器，那么仅 IP-HTTPS 可以用作转换协议。  
  
-   至少一个域控制器。 远程访问服务器和 DirectAccess 客户端必须是域成员。  
  
-   如果不希望为 IP-HTTPS 或网络位置服务器使用自签名证书，或者如果希望使用客户端证书进行客户端 IPsec 身份验证，则需要在服务器上安装证书颁发机构。  
  
### <a name="client-requirements"></a>客户端要求  
  
-   客户端计算机必须运行 Windows 10 或 Windows 8 或 Windows 7。  
  
### <a name="infrastructure-and-management-server-requirements"></a>基础结构和管理服务器要求  
  
-   在远程管理 DirectAccess 客户端计算机期间，客户端启动与管理服务器的通信，比如域控制器、System Center Configuration Server 以及健康注册机构 (HRA) 服务器。 这些服务器可提供 Windows 和防病毒更新以及网络访问保护 (NAP) 客户端合规性等服务。 开始远程访问部署之前，应该先部署所需的服务器。  
  
-   运行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 SP2 的 DNS 服务器是必需的。  
  
## <a name="BKMK_SOFT"></a>软件要求  
本方案的软件要求包括以下各项：  
  
### <a name="server-requirements"></a>服务器要求  
  
-   远程访问客户端必须使用 V.90 调制解调器。 可以将服务器部署在内部网络边缘，或边缘防火墙或其他设备的后面。  
  
-   如果远程访问服务器位于边缘防火墙或 NAT 设备的后面，则该设备必须配置为允许远程访问服务器进出流量。  
  
-   部署远程访问服务器的人员需要具有服务器本地管理员权限和域用户权限。 此外，管理员需要有用于 DirectAccess 部署的 GPO 的权限。 若要利用将 DirectAccess 部署局限在移动计算机的功能，需要在域控制器上有创建 WMI 筛选器的域管理员权限。  
  
-   如果网络位置服务器不在远程访问服务器上，则需要单独的服务器来运行它。  
  
### <a name="remote-access-client-requirements"></a>远程访问客户端要求  
  
-   DirectAccess 客户端必须是域成员。 包含客户端的域可属于远程访问服务器所在的同一林中，或者它们可具有与远程访问服务器林或域的双向信任。  
  
-   包含配置为 DirectAccess 客户端的的计算机需要 Active Directory 安全组。 计算机不应该包括在多个包括 DirectAccess 客户端的安全组中。 如果客户端包括在多个组中，客户端请求的名称解析不会按预期方式工作。  
  

