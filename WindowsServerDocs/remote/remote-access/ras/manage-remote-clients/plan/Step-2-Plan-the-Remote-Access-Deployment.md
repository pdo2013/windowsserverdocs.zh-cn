---
title: 步骤2规划远程访问部署
description: 本主题是在 Windows Server 2016 中远程管理 DirectAccess 客户端的指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 131520f567da6529e342229a0f6965d3223f928b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404575"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>步骤2规划远程访问部署

>适用于：Windows Server（半年频道）、Windows Server 2016

规划要用于设置单个远程访问服务器以远程管理 DirectAccess 客户端的基础结构后，就可以计划远程访问设置向导将使用的设置。  
  
> [!NOTE]  
> 在继续执行这些任务之前，请参阅 @no__t 0Step 1：规划远程访问基础结构 @ no__t。  
  
|任务|描述|  
|----|--------|  
|[规划客户端部署策略](#plan-a-client-deployment-strategy)|确定将配置为 DirectAccess 客户端的托管服务器。|  
|[规划远程访问服务器部署策略](#plan-a-remote-access-server-deployment-strategy)|规划如何部署远程访问服务器。|  
|[规划基础结构服务器的配置](#plan-the-infrastructure-servers-configurations)|在远程访问部署中规划基础结构服务器，包括 DirectAccess 网络位置服务器、DNS 服务器和 DirectAccess 管理服务器。|  
  
## <a name="plan-a-client-deployment-strategy"></a>规划客户端部署策略  
规划客户端部署时，要作出三个决策：  
  
1.  DirectAccess 仅可用于移动计算机，还是可用于指定安全组中的每台计算机？  
  
    当你在 DirectAccess 客户端安装向导中配置 DirectAccess 客户端时，你可以选择仅允许指定安全组中的移动计算机使用 DirectAccess 连接到服务器。 如果你限制了移动计算机的访问权限，则远程访问将自动配置 WMI 筛选器以确保 DirectAccess 客户端 GPO 仅应用于指定安全组中的移动计算机。 远程访问管理员需要创建或修改组策略 WMI 筛选器的权限，以启用此设置。  
  
2.  哪些安全组将包含 DirectAccess 客户端计算机？  
  
    DirectAccess 设置包含在 DirectAccess 客户端组策略对象（GPO）中。 GPO 将应用于属于你在 DirectAccess 客户端安装向导中指定的安全组的计算机。 可以指定包含在任何受支持的域中的安全组。
  
    在配置远程访问之前，需要创建安全组。 完成远程访问部署后，你可以将计算机添加到安全组。 但是，如果添加的客户端计算机位于与安全组不同的域中，则客户端 GPO 将不会应用于这些客户端。 例如，如果在域 A 中为 DirectAccess 客户端创建 SG1，之后将客户端从域 B 添加到该组，则客户端 GPO 将不会应用于域 B 中的客户端。  
  
    若要避免此问题，请为每个包含客户端计算机的域创建一个新的客户端安全组。 或者，如果不想创建新的安全组，请使用新域的新 GPO 名称运行**Add-daclient** Windows PowerShell cmdlet。  
  
3.  你将为 DirectAccess 网络连接助手配置哪些设置？  
  
    DirectAccess 网络连接助手在客户端计算机上运行，并向最终用户提供有关 DirectAccess 连接的其他信息。 在 DirectAccess 客户端安装向导中，你可以进行下列配置：  
  
    -   **连接性验证**  
  
        将创建默认 Web 探测，客户端可将其用于验证到内部网络的连接性。 默认名称为`https://directaccess-WebProbeHost.<domain_name>`。 应在 DNS 中手动注册该名称。 可以通过 HTTP 或 PING 创建使用其他 web 地址的其他连接性验证程序。 对于每个连接性验证程序，都必须存在 DNS 条目。  
  
    -   **技术支持电子邮件地址**  
  
        如果最终用户遇到 DirectAccess 连接问题，则他们可以将包含诊断信息的电子邮件发送到远程访问管理员，该管理员可以对问题进行故障排除。  
  
    -   **DirectAccess 连接名称**  
  
        你可以指定 DirectAccess 连接名称，以帮助最终用户在其计算机上标识 DirectAccess 连接。  
  
    -   **允许 DirectAccess 客户端使用本地名称解析**  
  
        客户端需要在本地解析名称的方法。 如果你允许 DirectAccess 客户端使用本地名称解析，则最终用户可以使用本地 DNS 服务器解析名称。 当最终用户选择使用本地 DNS 服务器进行名称解析时，DirectAccess 不会将对单标签名称的解析请求发送到内部企业 DNS 服务器。 它使用本地名称解析（通过使用链路本地多播名称解析（LLMNR）和 TCP/IP 上的 NetBios 协议）。  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>规划远程访问服务器部署策略  
计划部署远程访问服务器时需要做出的决策包括：  
  
-   **网络拓扑**  
  
    部署远程访问服务器时，有两种可用的拓扑：  
  
    -   **两个适配器**：使用两个网络适配器时，可以使用一个直接连接到 Internet 的网络适配器以及另一个连接到内部网络的网络适配器来配置远程访问。 或者，将服务器安装在边缘设备（如防火墙或路由器）的后面。 在此配置中，一个网络适配器连接到外围网络，另一个连接到内部网络。  
  
    -   **单个网络适配器**：在此配置中，远程访问服务器安装在边缘设备（如防火墙或路由器）的后面。 网络适配器连接到内部网络。  

-   **网络适配器**  
  
    远程访问服务器安装向导会自动检测在远程访问服务器上配置的网络适配器。 必须确保已选择正确的适配器。  
  
-   **Ip-https 证书**  
  
    远程访问服务器安装向导会自动检测适合 IP-HTTPS 连接的证书。 你选择的证书的使用者名称必须与 ConnectTo 地址相匹配。 如果你使用的是自签名证书，则可以选择使用远程访问服务器自动创建的证书。  
  
-   **IPv6 前缀**  
  
    如果远程访问服务器安装向导检测到已在网络适配器上部署 IPv6，则它会自动填充用于内部网络的 IPv6 前缀、要分配到 DirectAccess 客户端计算机的 IPv6 前缀，以及要分配到 VPN 客户端计算机的 IPv6 前缀。 如果为本机 IPv6 或 ISATAP 基础结构自动生成的前缀不正确，你必须手动更改它们。  
  
-   **身份验证**  
  
    您可以选择以下方法之一，对远程访问服务器的 DirectAccess 客户端进行身份验证：  
  
    -   **用户身份验证**：你可以使用户使用 Active Directory 凭据或双重身份验证进行身份验证。  
  
    -   **计算机身份验证**：你可以将计算机身份验证配置为使用证书。 也可以将远程访问服务器用作 Kerberos 身份验证的代理，而无需证书。 
  
    -   **Windows 7 客户端**默认情况下，运行 Windows 7 的客户端计算机无法连接到运行 Windows Server 2012 的远程访问部署。 如果你的组织中的客户端运行需要远程访问内部资源的 Windows 7，则你可以允许它们进行连接。 任何你希望允许访问内部资源的客户端计算机都必须是你在 DirectAccess 客户端安装向导中指定的安全组的成员。  
  
        > [!NOTE]  
        > 允许运行 Windows 7 的客户端使用 DirectAccess 进行连接需要使用计算机证书身份验证。  
  
-   **VPN 配置**  
  
    在配置远程访问之前，请决定是否要向远程客户端提供 VPN 访问权限。 如果组织中的客户端计算机不支持 DirectAccess 连接（例如，它们是不受管理的，或者运行不支持 DirectAccess 的操作系统），则应提供 VPN 访问。 远程访问服务器安装向导允许你配置如何分配 IP 地址（通过使用 DHCP 或从静态地址池），以及如何对 VPN 客户端进行身份验证（通过使用 Active Directory 或 RADIUS 服务器）。  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>规划基础结构服务器的配置  
远程访问需要三种类型的基础结构服务器：  
  
-   **网络位置服务器**  
  
-   **DNS 服务器** 
  
-   **管理服务器** 
  
## <a name="see-also"></a>请参阅  
  
-   [步骤 1：规划远程访问基础结构](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


