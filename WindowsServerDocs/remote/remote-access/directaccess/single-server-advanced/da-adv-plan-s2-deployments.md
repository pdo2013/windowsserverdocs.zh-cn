---
title: 步骤2规划高级 DirectAccess 部署
description: 本主题是 "使用 Windows Server 2016 的高级设置部署单个 DirectAccess 服务器" 指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3bba28d4-23e2-449f-8319-7d2190f68d56
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b093c4cbf5ceb06e84d5e07c8735106797932bc1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404929"
---
# <a name="step-2-plan-advanced-directaccess-deployments"></a>步骤2规划高级 DirectAccess 部署

>适用于：Windows Server（半年频道）、Windows Server 2016

规划 DirectAccess 基础结构后，使用 IPv4 和 IPv6 在单个服务器上部署高级 DirectAccess 的下一步是规划远程访问设置向导的设置。  
  
|任务|描述|  
|----|--------|  
|[2.1 规划客户端部署](#21-plan-for-client-deployment)|规划如何允许客户端计算机使用 DirectAccess 进行连接。 确定将哪些托管的计算机配置为 DirectAccess 客户端，并规划在客户端计算机上部署网络连接助手或 DirectAccess 连接助手。|  
|[2.2 规划 DirectAccess 服务器部署](#22-plan-for-directaccess-server-deployment)|规划如何部署 DirectAccess 服务器。|  
|[2.3 规划基础结构服务器](#23-plan-infrastructure-servers)|为 DirectAccess 部署规划基础结构服务器，包括 DirectAccess 网络位置服务器、域名系统 (DNS) 服务器和 DirectAccess 管理服务器。|  
|[2.4 规划应用程序服务器](#24-plan-application-servers)|规划 IPv4 和 IPv6 应用程序服务器，并可以考虑是否需要 DirectAccess 客户端计算机和内部应用程序服务器之间的端到端身份验证。|  
|[2.5 计划 DirectAccess 和第三方 VPN 客户端](#25-plan-directaccess-and-third-party-vpn-clients)|在使用第三方 VPN 客户端部署 DirectAccess 时，可能需要设置注册表值以使两个远程访问解决方案能够无缝共存。|  
  
## <a name="21-plan-for-client-deployment"></a>2.1 规划客户端部署  
规划客户端部署时，要作出三个决策：  
  
1.  DirectAccess 仅对移动计算机可用，还是对任何一台计算机都可用？  
  
    在 DirectAccess 客户端安装向导中配置 DirectAccess 客户端时，你可以选择仅允许指定安全组中的移动计算机使用 DirectAccess 进行连接。 如果你限制了移动计算机的访问权限，则远程访问将自动配置 WMI 筛选器以确保 DirectAccess 客户端 GPO 仅应用于指定安全组中的移动计算机。 远程访问管理员需要“创建或修改”安全权限，才能创建或修改组策略对象 (GPO) WMI 筛选器，从而启用此设置。  
  
2.  哪些安全组将包含 DirectAccess 客户端计算机？  
  
    DirectAccess 客户端设置包含在 DirectAccess 客户端 GPO 中。 GPO 将应用于属于你在 DirectAccess 客户端安装向导中指定的安全组的计算机。 可以指定在任一支持的域中包含安全组。 有关详细信息，请参阅[1.7 计划 Active Directory 域服务](da-adv-plan-s1-infrastructure.md#17-plan-active-directory-domain-services)。  
  
    在配置 DirectAccess 之前，应该创建安全组。 你可以在完成 DirectAccess 部署后，将计算机添加到安全组，但如果你要添加安全组之外的其他域中的客户端计算机，则客户端 GPO 将不会应用于这些客户端。 例如，如果在域 A 中为 DirectAccess 客户端创建 SG1，之后将客户端从域 B 添加到该组，则客户端 GPO 不会应用于来自域 B 的客户端。若要避免此问题，请为每个包含 DirectAccess 客户端计算机的域创建一个新的客户端安全组。 另外，如果你不希望创建新的安全组，则可使用新域的新 GPO 名称运行 **Add-DAClient**。  
  
3.  将为网络连接助手配置哪些设置？  
  
    网络连接助手在客户端计算机上运行，并向最终用户提供有关 DirectAccess 连接的其他信息。 在 DirectAccess 客户端安装向导中，你可以进行下列配置：  
  
    -   **连接性验证**  
  
        将创建默认 Web 探测，客户端可将其用于验证到内部网络的连接性。 默认名称为：  
  
        https://directaccess-WebProbeHost.<domain_name>  
  
        应在 DNS 中手动注册该名称。 你可以通过 HTTP 或 **ping** 使用其他 Web 地址来创建其他的连接性验证程序。 对于每个连接性验证程序，都必须存在 DNS 条目。  
  
    -   **咨询台电子邮件地址**  
  
        如果最终用户遇到 DirectAccess 连接问题，他们可以向 DirectAccess 管理员发送包含诊断信息的电子邮件以解决问题。  
  
    -   **DirectAccess 连接名称**  
  
        指定 DirectAccess 连接名称以帮助最终用户在其计算机上识别 DirectAccess 连接。  
  
    -   **允许 DirectAccess 客户端使用本地名称解析**  
  
        客户端需要一种在本地解析名称的方法。 如果你允许 DirectAccess 客户端使用本地名称解析，则最终用户可以使用本地 DNS 服务器解析名称。 当最终用户选择使用本地 DNS 服务器进行名称解析时，DirectAccess 不会将单标签名称的解析请求发送到企业内部的 DNS 服务器。 它将改用本地名称解析（通过使用链路本地多播名称解析 (LLMNR) 和 TCP/IP 上的 NetBIOS 协议）。  
  
## <a name="22-plan-for-directaccess-server-deployment"></a>2.2 规划 DirectAccess 服务器部署  
当你打算部署 DirectAccess 服务器时，请考虑以下决策：  
  
-   **网络拓扑**  
  
    部署 DirectAccess 服务器时可使用以下两种拓扑：  
  
    -   **两个网络适配器** 具有两个网络适配器时，可以将 DirectAccess 配置为一个网络适配器直接连接到 Internet，另一个网络适配器连接到内部网络。 或者将该服务器安装在边缘设备（如防火墙或路由器）的后面。 在这一配置中，一个网络适配器连接到外围网络，另一个连接到内部网络。  
  
    -   **一个网络适配器** 在这一配置中，DirectAccess 服务器安装在边缘设备（如防火墙或路由器）的后面。 网络适配器连接到内部网络。  
  
    有关为部署选择拓扑的详细信息，请参阅[1.1 计划网络拓扑和设置](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings)。  
  
-   **ConnectTo 地址**  
  
    客户端计算机使用 ConnectTo 地址连接到 DirectAccess 服务器。 所选择的地址必须与你为 IP-HTTPS 连接部署的 IP-HTTPS 证书的使用者名称相匹配，并且必须在公用 DNS 中可用。  
  
-   **网络适配器**  
  
    远程访问服务器安装向导会自动检测 DirectAccess 服务器上配置的网络适配器。 必须确保已选择正确的适配器。  
  
-   **Ip-https 证书**  
  
    远程访问服务器安装向导会自动检测适合 IP-HTTPS 连接的证书。 你选择的证书的使用者名称必须与 ConnectTo 地址相匹配。 如果你要使用自签名证书，则可以选择使用由远程访问服务器自动创建的证书。  
  
-   **IPv6 前缀**  
  
    如果远程访问服务器安装向导检测到已在网络适配器上部署 IPv6，则它会自动填充用于内部网络的 IPv6 前缀、要分配到 DirectAccess 客户端计算机的 IPv6 前缀，以及要分配到 VPN 客户端计算机的 IPv6 前缀。 如果为本机 IPv6 基础结构自动生成的前缀不正确，则你必须手动更改它们。 有关详细信息，请参阅[1.1 计划网络拓扑和设置](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings)。  
  
-   **身份验证**  
  
    确定 DirectAccess 客户端如何对 DirectAccess 服务器进行身份验证：  
  
    -   **用户身份验证** 你可以使用户使用 Active Directory 凭据或双重身份验证进行身份验证。 有关使用双重身份验证进行身份验证的详细信息，请参阅[使用 OTP 身份验证部署远程访问](https://technet.microsoft.com/library/hh831379.aspx)。  
  
    -   **计算机身份验证**。 你可以将计算机身份验证配置为使用证书，或配置为将 DirectAccess 服务器用作代表客户端的 Kerberos 代理。 有关详细信息，请参阅[1.3 计划证书要求](da-adv-plan-s1-infrastructure.md#13-plan-certificate-requirements)。  
  
    -   **Windows 7 客户端**。 默认情况下，运行 Windows 7 的客户端计算机无法连接到 Windows Server 2012 R2 或 Windows Server 2012 DirectAccess 部署。 如果你的组织中有运行 Windows 7 的客户端，并且它们需要远程访问内部资源，你可以允许它们进行连接。 任何你希望允许访问内部资源的客户端计算机都必须是你在 DirectAccess 客户端安装向导中指定的安全组的成员。  
  
        > [!NOTE]  
        > 如果允许运行 Windows 7 的客户端使用 DirectAccess 进行连接，则需要使用计算机证书身份验证。  
  
-   **VPN 配置**  
  
    配置 DirectAccess 之前，请先确定是否要对不支持 DirectAccess 的远程客户端提供 VPN 访问。 如果你的组织中包含不支持 DirectAccess 连接的客户端计算机（因为它们是非托管的，或者它们运行不支持 DirectAccess 的操作系统），则你应该提供 VPN 访问。 远程访问服务器安装向导允许你配置如何分配 IP 地址（通过使用 DHCP 或从静态地址池中分配），以及如何对 VPN 客户端进行身份验证（通过使用 Active Directory 或远程身份验证拨入服务 (RADIUS) 服务器）。  
  
## <a name="23-plan-infrastructure-servers"></a>2.3 规划基础结构服务器  
DirectAccess 需要三种类型的基础结构服务器：  
  
-   **DNS 服务器**。 有关详细信息，请参阅 [1.4 规划 DNS 要求](da-adv-plan-s1-infrastructure.md#14-plan-dns-requirements)  
  
-   **网络位置服务器**。 有关详细信息，请参阅 [1.5 规划网络位置服务器](da-adv-plan-s1-infrastructure.md#15-plan-the-network-location-server)  
  
-   **管理服务器**。 有关详细信息，请参阅 [1.6 规划管理服务器](da-adv-plan-s1-infrastructure.md#16-plan-management-servers)  
  
## <a name="24-plan-application-servers"></a>2.4 规划应用程序服务器  
应用程序服务器是在企业网络上由客户端计算机通过 DirectAccess 连接进行访问的服务器。 通过将应用程序服务器添加到安全组中对其进行标识。 然后将应用程序服务器 GPO 应用到该组中的服务器。  
  
> [!NOTE]  
> 仅当你需要进行端到端身份验证和加密时，才需将应用程序服务器添加到安全组中。  
  
你可以要求在 DirectAccess 客户端与所选的内部应用程序服务器之间进行端到端身份验证和加密。 在配置端到端身份验证时，DirectAccess 客户端使用 IPsec 传输策略。 这一策略要求 IPsec 会话的身份验证和通信保护在指定的应用程序服务器上终止。 在这种情况下，远程访问服务器将经过身份验证和受保护的 IPsec 会话转发到应用程序服务器。  
  
默认情况下，当你将身份验证扩展到应用程序服务器时，将在 DirectAccess 客户端和应用程序服务器之间对数据负载进行加密。 你可以选择不加密通信，并仅使用身份验证。 但是，这不如使用身份验证和加密，并且仅支持运行 Windows Server 2008 R2 或 Windows Server 2012 操作系统的应用程序服务器。  
  
## <a name="25-plan-directaccess-and-third-party-vpn-clients"></a>2.5 规划 DirectAccess 和第三方 VPN 客户端  
某些第三方 VPN 客户端不会在网络连接文件夹中创建连接。 这可能会导致 DirectAccess 认为：在建立 VPN 连接且存在到 Intranet 的连接时，它并不具有 Intranet 连接。 当第三方 VPN 客户端通过将其接口定义为网络设备接口规范 (NDIS) 端点类型来对其接口进行注册时，将发生这种情况。 你可以通过在 DirectAccess 客户端上将以下注册表值设置为 1，使这些类型的 VPN 客户端共存：  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\ShowDomainEndpointInterfaces （REG_DWORD）**  
  
某些第三方 VPN 客户端使用拆分隧道配置，这将允许 VPN 客户端计算机直接访问 Internet，而无需将通信通过 VPN 连接发送到 Intranet。  
  
通常，拆分隧道配置会将 VPN 客户端上的默认网关设置保留为未配置或全为零 (0.0.0.0)。 通过建立到 Intranet 的成功 VPN 连接并使用 Ipconfig.exe 命令行工具查看生成的配置，你可以对此进行确认。  
  
如果 VPN 连接将其默认网关列为空或全为零 (0.0.0.0)，则说明你的 VPN 客户端采用这种方式配置。 默认情况下，DirectAccess 客户端不会识别拆分隧道配置。 若要配置 DirectAccess 客户端以检测这些类型的 VPN 客户端配置并与之共存，请将以下注册表值设置为 1：  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\Internet\ EnableNoGatewayLocationDetection （REG_DWORD）**  
  
## <a name="previous-step"></a>上一步  
  
-   [步骤 1：规划 DirectAccess 基础结构 @ no__t-0  
  


