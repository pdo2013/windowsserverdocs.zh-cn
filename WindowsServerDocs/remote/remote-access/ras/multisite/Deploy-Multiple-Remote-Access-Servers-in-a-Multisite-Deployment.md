---
title: 在多站点部署中部署多台远程访问服务器
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3091c770fb9b207d82deaa571970bfeb44d17ee3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875878"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>在多站点部署中部署多台远程访问服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 将 DirectAccess 和远程访问服务 (RAS) VPN 合并到单个远程访问角色。 远程访问可在许多个企业方案中部署。 本概述介绍部署多站点配置中的远程访问服务器的企业方案。  
  
## <a name="BKMK_OVER"></a>应用场景说明  
在多站点部署两个或多个远程访问服务器或服务器群集部署和配置为在单个位置，或处于不同地理位置的不同入口点。 部署在单个位置中的多个入口点，可以为服务器冗余，或使用现有的网络体系结构的远程访问服务器的对齐方式。 按地理位置部署可确保有效地利用资源，如远程客户端计算机可以连接到内部网络资源使用离其最近的入口点。 可分发并通过外部全局负载均衡器均衡在多站点部署之间的流量。  
  
多站点部署中支持运行 Windows 10、 Windows 8 或 Windows 7 的客户端计算机。 自动运行 Windows 10 或 Windows 8 客户端计算机标识的入口点，或用户可以手动选择的入口点。 自动分配以下优先级顺序发生：  
  
1.  使用由用户手动选择的入口点。  
  
2.  使用由外部全局负载均衡器，如果其中一个部署的入口点。  
  
3.  使用由客户端计算机使用自动探测机制标识的最近入口点。  
  
必须在每个入口点，手动启用运行 Windows 7 的客户端支持和不支持这些客户端的入口点的选择。  
  
## <a name="prerequisites"></a>系统必备  
在开始部署此方案之前，请查看此列表以了解重要要求：  
  
-   [部署单个 DirectAccess 服务器使用高级设置](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)多站点部署之前，必须先部署。  
  
-   Windows 7 客户端将始终连接到特定站点。 它们将无法再连接到最近的站点基于 （不同于 Windows 10、 8 或 8.1 客户端） 的客户端的位置。  
  
-   不支持在 DirectAccess 管理控制台或 PowerShell cmdlet 之外更改策略。  
  
-   必须部署公钥基础结构。  
  
    有关详细信息，请参阅：[测试实验室指南微型模块：适用于 Windows Server 2012 的基本 PKI。](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   企业网络必须启用 IPv6。 如果你使用的是 ISATAP，应该将其删除并使用本机 IPv6。  
  
## <a name="in-this-scenario"></a>本方案内容  
多站点部署方案包括多个步骤：  
  
1. [部署单个 DirectAccess 服务器使用高级设置](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 设置多站点部署之前，必须部署单个远程访问服务器使用高级设置。  
  
2. [计划多站点部署](plan/Plan-a-Multisite-Deployment.md)。 若要从一台服务器的多站点部署的生成号额外的规划步骤是必需的包括法规遵从性与多站点的先决条件和规划 Active Directory 安全组、 组策略对象 (Gpo)、 DNS 和客户端设置。  
  
3. [配置多站点部署](configure/Configure-a-Multisite-Deployment.md)。 这包括多个配置步骤，包括 Active Directory 基础结构，配置现有的远程访问服务器，并将其添加多台远程访问服务器作为入口点到多站点部署的准备。  
  
4. [对多站点部署进行故障排除](troubleshoot/Troubleshoot-a-Multisite-Deployment.md)。 此故障排除部分介绍多站点部署中部署远程访问时可能出现的最常见错误的数。
  
## <a name="BKMK_APP"></a>实际应用程序  
多站点部署提供以下功能：  
  
-   改进了的性能的多站点部署允许客户端计算机访问内部资源使用远程访问连接使用的最接近最适合的入口点。 客户端访问内部资源高效，并提高客户端请求 Internet 路由通过 DirectAccess 的速度。 可以使用外部全局负载均衡器均衡的入口点之间的流量。  
  
-   轻松的管理 Multisite 允许管理员将对齐到 Active Directory 站点部署中，提供简化的体系结构的远程访问部署。 共享的设置可以轻松设置个入口点服务器或群集中。 可以从任何部署，或远程使用远程服务器管理工具 (RSAT) 中的服务器管理远程访问设置。 此外，可以从单个远程访问管理控制台监视整个多站点部署。  
  
## <a name="BKMK_NEW"></a>在此方案中包括角色和功能  
下表列出了角色和功能在此方案中使用。  
  
|角色/功能|如何支持本方案|  
|---------|-----------------|  
|远程访问角色|该角色可使用服务器管理器控制台加以安装和卸载。 它包括 DirectAccess（以前是 Windows Server 2008 R2 中的功能）以及路由和远程访问服务 (RRAS)（以前是网络策略和访问服务 (NPAS) 服务器角色项下的角色服务）。 远程访问角色由以下两个组件组成：<br /><br />-DirectAccess 以及路由和远程访问服务 (RRAS) VPN DirectAccess 和 VPN 由远程访问管理控制台中一起管理。<br />的在传统路由和远程访问控制台中管理 RRAS 路由 — RRAS 路由功能。<br /><br />依赖关系如下所示：<br /><br />Internet 信息服务 (IIS) Web 服务器-此功能是所需配置网络位置服务器和默认 web 探测。<br />的远程访问服务器上的本地帐户的 Windows 内部 Database-Used。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-如果远程访问角色安装，并且支持远程管理控制台用户界面它是默认情况下，远程访问服务器上安装。<br />-它可以在不运行远程访问服务器角色的服务器上选择性地安装。 在这种情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />-远程访问 GUI 和命令行工具<br />的 Windows PowerShell 远程访问模块<br /><br />依赖项包括：<br /><br />组策略管理控制台<br />-RAS 连接管理器管理工具包 (CMAK)<br />-   Windows PowerShell 3.0<br />图形管理工具和基础结构|  
  
## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  
  
-   若要收集在多站点部署中的至少两个远程访问计算机。   
  
-   为测试该方案，至少一台计算机运行 Windows 8 并配置为 DirectAccess 客户端是必需的。 若要测试的方案运行 Windows 7 的客户端至少一台计算机运行 Windows 7 是必需的。  
  
-   若要在入口点服务器负载平衡流量，第三方外部全局负载均衡器是必需的。  
  
## <a name="BKMK_SOFT"></a>软件要求  
本方案的软件要求包括以下各项：  
  
-   对单台服务器部署的软件要求。  
  
-   除了单个服务器的软件要求有许多 multisite-特定要求：  
  
    -   IPsec 身份验证要求中必须使用 IPsec 计算机证书身份验证部署多站点部署 DirectAccess。 不支持执行 IPsec 身份验证的远程访问服务器用作 Kerberos 代理的选项。 部署 IPsec 证书所需的内部 CA。  
  
    -   必须由 CA 颁发 IP-HTTPS 和网络位置服务器要求的所需的证书的 IP-HTTPS 和网络位置服务器。 不支持使用自动颁发并由远程访问服务器自签名证书的选项。 证书可以由内部 CA 或由第三方外部 CA 颁发。  
  
    -   Active Directory 要求-是必需的至少一个 Active Directory 站点。 远程访问服务器应位于的站点。 对于更快的更新时间，建议，每个站点都有一个可写域控制器，但这不是强制性。  
  
    -   安全组的要求要求如下所示：  
  
        -   单独的安全组是必需的所有域中的所有 Windows 8 客户端计算机。 建议创建唯一的安全组的每个域这些客户端。  
  
        -   每个入口点配置为支持 Windows 7 客户端需要包含 Windows 7 计算机的唯一安全组。 建议在每个域中有每个入口点的唯一安全组。  
  
        -   计算机不应该包括在多个包括 DirectAccess 客户端的安全组中。 如果客户端包括在多个组中，客户端请求的名称解析不会按预期方式工作。  
  
    -   可以在配置远程访问之前手动创建或在远程访问部署过程中自动创建 GPO 要求 Gpo。 要求如下所示：  
  
        -   需要为每个域唯一的客户端 GPO。  
  
        -   服务器 GPO 是必需的每个入口点，入口点所在的域中。 因此如果多个入口点位于同一域中，将有多个服务器域中的 Gpo （一个用于每个入口点）。  
  
        -   唯一的 Windows 7 客户端 GPO 都需要为每个域的 Windows 7 客户端支持启用每个入口点。  
  
## <a name="KnownIssues"></a>已知的问题  
配置多站点方案时，以下是已知问题：  
  
-   **位于同一 IPv4 子网的多个入口点**。 添加多个入口点位于同一 IPv4 子网将导致一个 IP 地址冲突消息后，并按预期方式，将不会配置的入口点的 DNS64 地址。 未在公司网络上的服务器的内部接口上部署 IPv6 时，将出现此问题。 若要防止此问题当前和未来的所有远程访问服务器上运行以下 Windows PowerShell 命令：  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   如果为 DirectAccess 客户端连接到远程访问服务器指定的公共地址包含在 NRPT 后缀，DirectAccess 可能无法按预期方式工作。 确保 NRPT 免除条目的公共名称。 在多站点部署中，应为所有入口点的公共名称添加免除。 请注意，如果强制隧道启用自动添加这些例外。 如果强制隧道处于禁用状态，则会删除它们。  
  
-   使用 Windows PowerShell cmdlet 时**禁用 DAMultiSite**、 WhatIf 和 Confirm 参数已不起作用，并将禁用多站点和 Windows 7 Gpo 将被删除。  
  
-   时 DCA 使用多站点部署中的 Windows 7 客户端升级到 Windows 8 中，网络连接助手将不起作用。 可以通过修改 Windows 7 Gpo 使用以下 Windows PowerShell cmdlet 在客户端升级之前解决此问题：  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    在的已升级客户端，然后将客户端计算机移动到 Windows 8 安全组。  
  
-   修改域控制器设置，请使用 Windows PowerShell cmdlet 时**Set-daentrypointdc**、 如果指定的 ComputerName 参数中不是最后一个添加到多站点入口点的远程访问服务器部署中，警告将显示，该值指示指定的服务器不会更新直到下一次策略刷新。 可以使用查看未更新的实际服务器**配置状态**中**仪表板**的**远程访问管理控制台**。 这不会导致任何功能问题，但是，可以运行**gpupdate /force**未更新以立即更新配置状态的服务器上。  
  
-   多站点部署在仅使用 IPv4 的公司网络，更改内部网络 IPv6 前缀还更改 DNS64 地址，但不会更新防火墙规则，允许到 DNS64 服务的 DNS 查询上的地址。 若要解决此问题，请更改内部网络 IPv6 前缀后运行以下 Windows PowerShell 命令：  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   如果删除了 ISATAP 主机的入口点时存在现有的 ISATAP 基础结构时，已部署 DirectAccess DNS64 服务的 IPv6 地址将不再从 NRPT 中的所有 DNS 后缀的 DNS 服务器地址。  
  
    若要解决此问题，请在**基础结构服务器安装程序**向导**DNS**页上，删除已修改的 DNS 后缀，通过单击具有正确的DNS服务器地址，再次添加它们**检测**上**的 DNS 服务器地址**对话框。  
  


