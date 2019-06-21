---
title: 步骤 2 规划远程访问部署
description: 本主题是指南的在 Windows Server 2016 中远程管理 DirectAccess 客户端的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 78303bbdd29819389944348a279fb4a52f1570fb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282792"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>步骤 2 规划远程访问部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你想要用于设置远程管理 DirectAccess 客户端在单个远程访问服务器的基础结构规划后，已准备好规划远程访问设置向导将使用的设置。  
  
> [!NOTE]  
> 在继续这些任务之前，请参阅[步骤 1:规划远程访问基础结构](Step-1-Plan-the-Remote-Access-Infrastructure.md)。  
  
|任务|描述|  
|----|--------|  
|[规划客户端部署策略](#plan-a-client-deployment-strategy)|确定将配置为 DirectAccess 客户端的托管服务器。|  
|[规划远程访问服务器部署策略](#plan-a-remote-access-server-deployment-strategy)|规划如何部署远程访问服务器。|  
|[规划基础结构服务器的配置](#plan-the-infrastructure-servers-configurations)|规划远程访问部署，包括 DirectAccess 网络位置服务器、 DNS 服务器和 DirectAccess 管理服务器中的基础结构服务器。|  
  
## <a name="plan-a-client-deployment-strategy"></a>规划客户端部署策略  
规划客户端部署时，要作出三个决策：  
  
1.  将 DirectAccess 可供移动计算机，或指定的安全组中的每台计算机？  
  
    当 DirectAccess 客户端安装向导中配置 DirectAccess 客户端时，您可以选择仅允许移动计算机使用 DirectAccess 连接到服务器的指定的安全组中。 如果你限制了移动计算机的访问权限，则远程访问将自动配置 WMI 筛选器以确保 DirectAccess 客户端 GPO 仅应用于指定安全组中的移动计算机。 远程访问管理员需要创建或修改组策略 WMI 筛选器的权限，以启用此设置。  
  
2.  哪些安全组将包含 DirectAccess 客户端计算机？  
  
    DirectAccess 设置包含在 DirectAccess 客户端组策略对象 (GPO)。 GPO 将应用于属于你在 DirectAccess 客户端安装向导中指定的安全组的计算机。 可以指定包含在任何受支持的域中的安全组。
  
    在配置远程访问之前，需要创建安全组。 在完成远程访问部署后，可以将计算机添加到安全组。 但是，如果添加位于不同的安全组的不同域的客户端计算机，则客户端 GPO 将不会应用于这些客户端。 例如，如果 SG1 是在域 A 中为 DirectAccess 客户端创建并在更高版本向此组添加客户端从域 B，则客户端 GPO 将不会应用到域 B 中的客户端  
  
    若要避免此问题，请创建包含客户端计算机的每个域的新客户端安全组。 或者，如果您不想要创建新的安全组，则运行**Add-daclient**替换为新域的新 gpo 名称的 Windows PowerShell cmdlet。  
  
3.  您将为 DirectAccess 网络连接助手配置哪些设置？  
  
    DirectAccess 网络连接助手在客户端计算机上运行并向最终用户提供有关 DirectAccess 连接的其他信息。 在 DirectAccess 客户端安装向导中，你可以进行下列配置：  
  
    -   **连接性验证程序**  
  
        将创建默认 Web 探测，客户端可将其用于验证到内部网络的连接性。 默认名称是`https://directaccess-WebProbeHost.<domain_name>`。 应在 DNS 中手动注册该名称。 您可以创建其他连接性验证程序，通过 HTTP 或 PING 使用其他 web 地址。 对于每个连接性验证程序，都必须存在 DNS 条目。  
  
    -   **帮助服务台电子邮件地址**  
  
        如果最终用户遇到 DirectAccess 连接问题，他们可以发送包含到远程访问管理员，他可以解决此问题的诊断信息的电子邮件。  
  
    -   **DirectAccess 连接名称**  
  
        可以指定一个 DirectAccess 连接名称，以帮助最终用户识别他们的计算机上的 DirectAccess 连接。  
  
    -   **允许 DirectAccess 客户端使用本地名称解析**  
  
        客户端需要本地名称解析的方法。 如果你允许 DirectAccess 客户端使用本地名称解析，则最终用户可以使用本地 DNS 服务器解析名称。 当最终用户选择使用本地 DNS 服务器进行名称解析时，则 DirectAccess 不向公司内部 DNS 服务器发送单标签名称解析的请求。 它改为使用本地名称解析 （通过使用链路本地多播名称解析 (LLMNR) 和 NetBios over TCP/IP 协议）。  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>规划远程访问服务器部署策略  
你需要进行规划以部署远程访问服务器时的决策包括：  
  
-   **网络拓扑**  
  
    有可用的两个拓扑，部署远程访问服务器时：  
  
    -   **两个适配器**:具有两个网络适配器，可以有一个直接连接到 Internet 的网络适配器配置远程访问和其他连接到内部网络。 或者，将服务器安装在边缘设备，例如防火墙或路由器后面或者。 在此配置一个网络适配器连接到外围网络和其他已连接到内部网络。  
  
    -   **单个网络适配器**:在此配置远程访问服务器安装在边缘设备，例如防火墙或路由器后面。 网络适配器连接到内部网络。  

-   **网络适配器**  
  
    远程访问服务器安装向导会自动检测远程访问服务器配置的网络适配器。 必须确保已选择正确的适配器。  
  
-   **IP-HTTPS 证书**  
  
    远程访问服务器安装向导会自动检测适合 IP-HTTPS 连接的证书。 你选择的证书的使用者名称必须与 ConnectTo 地址相匹配。 如果使用自签名的证书，你可以选择使用由远程访问服务器自动创建的证书。  
  
-   **IPv6 前缀**  
  
    如果远程访问服务器安装向导检测到已在网络适配器上部署 IPv6，则它会自动填充用于内部网络的 IPv6 前缀、要分配到 DirectAccess 客户端计算机的 IPv6 前缀，以及要分配到 VPN 客户端计算机的 IPv6 前缀。 如果为本机 IPv6 或 ISATAP 基础结构自动生成的前缀不正确，你必须手动更改它们。  
  
-   **身份验证**  
  
    可以选择用于远程访问服务器的 DirectAccess 客户端进行身份验证的以下方法之一：  
  
    -   **用户身份验证**:你可以使用户使用 Active Directory 凭据或双重身份验证进行身份验证。  
  
    -   **计算机身份验证**:你可以配置要使用证书的计算机身份验证。 或远程访问服务器可以充当 Kerberos 身份验证的代理不需要证书。 
  
    -   **Windows 7 客户端**默认情况下，运行 Windows 7 的客户端计算机无法连接到运行 Windows Server 2012 的远程访问部署。 如果您的客户端在组织中运行 Windows 7 需要远程访问内部资源，可以允许它们进行连接。 任何你希望允许访问内部资源的客户端计算机都必须是你在 DirectAccess 客户端安装向导中指定的安全组的成员。  
  
        > [!NOTE]  
        > 允许运行 Windows 7 通过 DirectAccess 连接的客户端要求使用计算机证书身份验证。  
  
-   **VPN 配置**  
  
    配置远程访问之前，确定是否要为远程客户端提供 VPN 访问。 如果必须在组织中不支持 DirectAccess 连接的客户端计算机，应提供 VPN 访问权限 （例如，它们不受管理或它们运行的操作系统不支持 DirectAccess 的）。 远程访问服务器安装向导可以配置如何分配 IP 地址 （通过使用 DHCP 或从静态地址池） 和如何 VPN 客户端进行身份验证 （通过使用 Active Directory 或 RADIUS 服务器）。  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>规划基础结构服务器的配置  
远程访问需要使用三种类型的基础结构服务器：  
  
-   **网络位置服务器**  
  
-   **DNS 服务器** 
  
-   **管理服务器** 
  
## <a name="see-also"></a>请参阅  
  
-   [步骤 1：规划远程访问基础结构](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


