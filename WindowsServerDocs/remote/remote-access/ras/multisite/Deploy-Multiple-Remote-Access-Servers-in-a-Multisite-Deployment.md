---
title: 在多站点部署中部署多台远程访问服务器
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da23f3082e1d97f1bcfbee7365b863d29ba2d020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404493"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>在多站点部署中部署多台远程访问服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 将 DirectAccess 和远程访问服务（RAS） VPN 合并到了单个远程访问角色中。 远程访问可在许多个企业方案中部署。 本概述介绍了在多站点配置中部署远程访问服务器的企业方案。  
  
## <a name="BKMK_OVER"></a>方案描述  
在多站点部署中，将两个或多个远程访问服务器或服务器群集部署并配置为一个位置或分散的地理位置中的不同入口点。 在一个位置部署多个入口点允许服务器冗余，或将远程访问服务器与现有的网络体系结构对齐。 按地理位置进行部署可确保有效地使用资源，因为远程客户端计算机可以使用最接近它们的入口点连接到内部网络资源。 可以通过外部全局负载均衡器来分发和平衡多站点部署中的流量。  
  
多站点部署支持运行 Windows 10、Windows 8 或 Windows 7 的客户端计算机。 运行 Windows 10 或 Windows 8 的客户端计算机自动标识入口点，或者用户可以手动选择一个入口点。 自动分配按以下优先级顺序进行：  
  
1.  使用用户手动选择的入口点。  
  
2.  如果部署了一个入口点，则使用由外部全局负载均衡器标识的入口点。  
  
3.  使用客户端计算机使用自动探测机制识别的最近的入口点。  
  
必须在每个入口点上手动启用对运行 Windows 7 的客户端的支持，并且不支持选择这些客户端的入口点。  
  
## <a name="prerequisites"></a>先决条件  
在开始部署此方案之前，请查看此列表以了解重要要求：  
  
-   必须先部署[具有高级设置的单个 DirectAccess 服务器，](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)然后才能部署多站点部署。  
  
-   Windows 7 客户端将始终连接到特定站点。 它们将无法根据客户端的位置（与 Windows 10、8或8.1 客户端不同）连接到最近的站点。  
  
-   不支持在 DirectAccess 管理控制台或 PowerShell cmdlet 之外更改策略。  
  
-   必须部署公钥基础结构。  
  
    有关详细信息，请参阅：@no__t 0Test 实验室指南微型模块：Windows Server 2012 的基本 PKI。 ](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   企业网络必须已启用 IPv6。 如果你使用的是 ISATAP，应该将其删除并使用本机 IPv6。  
  
## <a name="in-this-scenario"></a>本方案内容  
多站点部署方案包括多个步骤：  
  
1. [使用高级设置部署单个 DirectAccess 服务器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 设置多站点部署之前，必须先部署具有高级设置的单个远程访问服务器。  
  
2. [规划多站点部署](plan/Plan-a-Multisite-Deployment.md)。 若要从单个服务器生成多站点部署，需要执行许多额外的规划步骤，包括与多站点先决条件的符合性，以及规划 Active Directory 安全组、组策略对象（Gpo）、DNS 和客户端设置。  
  
3. [配置多站点部署](configure/Configure-a-Multisite-Deployment.md)。 这包括许多配置步骤，包括准备 Active Directory 基础结构、配置现有远程访问服务器，以及将多个远程访问服务器作为入口点添加到多站点部署。  
  
4. [排查多站点部署问题](troubleshoot/Troubleshoot-a-Multisite-Deployment.md)。 此故障排除部分介绍了在多站点部署中部署远程访问时可能出现的一些最常见的错误。
  
## <a name="BKMK_APP"></a>实用应用程序  
多站点部署提供以下内容：  
  
-   改进的性能-多站点部署允许客户端计算机使用远程访问访问内部资源使用最接近和最合适的入口点进行连接。 客户端有效地访问内部资源，并改善通过 DirectAccess 路由的客户端 Internet 请求的速度。 可以使用外部全局负载均衡器对入口点之间的流量进行均衡。  
  
-   易于管理-多站点允许管理员将远程访问部署与 Active Directory 站点部署保持一致，从而提供简化的体系结构。 可以在入口点服务器或群集上轻松设置共享设置。 可以从部署中的任何服务器管理远程访问设置，也可以使用远程服务器管理工具（RSAT）进行远程访问。 此外，可从单个远程访问管理控制台监视整个多站点部署。  
  
## <a name="BKMK_NEW"></a>此方案中包含的角色和功能  
下表列出了此方案中使用的角色和功能。  
  
|角色/功能|如何支持本方案|  
|---------|-----------------|  
|远程访问角色|该角色可使用服务器管理器控制台加以安装和卸载。 它包括 DirectAccess（以前是 Windows Server 2008 R2 中的功能）以及路由和远程访问服务 (RRAS)（以前是网络策略和访问服务 (NPAS) 服务器角色项下的角色服务）。 远程访问角色由以下两个组件组成：<br /><br />-DirectAccess 和路由和远程访问服务（RRAS） VPN-DirectAccess 和 VPN 在远程访问管理控制台中一起进行管理。<br />-RRAS 路由-RRAS 路由功能在旧版路由和远程访问控制台中进行管理。<br /><br />依赖关系如下所示：<br /><br />-Internet Information Services （IIS） Web 服务器-配置网络位置服务器和默认 Web 探测需要此功能。<br />-Windows 内部数据库-用于远程访问服务器上的本地记帐。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-在安装远程访问角色时，它默认安装在远程访问服务器上，并支持远程管理控制台用户界面。<br />-可选择将它安装在不运行远程访问服务器角色的服务器上。 在这种情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />-远程访问 GUI 和命令行工具<br />-适用于 Windows PowerShell 的远程访问模块<br /><br />依赖项包括：<br /><br />-组策略管理控制台<br />-RAS 连接管理器管理工具包（CMAK）<br />-Windows PowerShell 3。0<br />-图形管理工具和基础结构|  
  
## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  
  
-   至少两个要收集到多站点部署中的远程访问计算机。   
  
-   若要测试该方案，至少需要一台运行 Windows 8 的计算机并将其配置为 DirectAccess 客户端。 若要为运行 Windows 7 的客户端测试该方案，至少需要一台运行 Windows 7 的计算机。  
  
-   若要对入口点服务器之间的流量进行负载均衡，需要使用第三方外部全局负载均衡器。  
  
## <a name="BKMK_SOFT"></a>软件要求  
本方案的软件要求包括以下各项：  
  
-   对单台服务器部署的软件要求。  
  
-   除单台服务器的软件要求外，还存在许多特定于站点的要求：  
  
    -   IPsec 身份验证要求-在多站点部署中，必须使用 IPsec 计算机证书身份验证部署 DirectAccess。 不支持使用远程访问服务器作为 Kerberos 代理执行 IPsec 身份验证的选项。 部署 IPsec 证书需要内部 CA。  
  
    -   Ip-https 和网络位置服务器要求-ip-https 和网络位置服务器所需的证书必须由 CA 颁发。 不支持使用自动颁发并由远程访问服务器自签名的证书的选项。 证书可以由内部 CA 或第三方外部 CA 颁发。  
  
    -   Active Directory 要求-至少需要一个 Active Directory 站点。 远程访问服务器应该位于站点中。 为了更快地更新时间，建议每个站点都有可写域控制器，尽管这不是必需的。  
  
    -   安全组要求-要求如下：  
  
        -   所有域中的所有 Windows 8 客户端计算机都需要一个安全组。 建议为每个域创建一个唯一的客户端安全组。  
  
        -   对于配置为支持 Windows 7 客户端的每个入口点，都需要一个包含 Windows 7 计算机的唯一安全组。 建议为每个域中的每个入口点使用唯一的安全组。  
  
        -   计算机不应该包括在多个包括 DirectAccess 客户端的安全组中。 如果客户端包括在多个组中，客户端请求的名称解析不会按预期方式工作。  
  
    -   GPO 要求-可以在配置远程访问之前手动创建 Gpo，或在远程访问部署过程中自动创建 Gpo。 要求如下所示：  
  
        -   每个域都需要唯一的客户端 GPO。  
  
        -   在入口点所在的域中，每个入口点都需要服务器 GPO。 因此，如果多个入口点位于同一域中，则域中会有多个服务器 Gpo （每个入口点一个）。  
  
        -   对于每个域启用了每个 Windows 7 客户端支持的入口点，都需要唯一的 Windows 7 客户端 GPO。  
  
## <a name="KnownIssues"></a>已知问题  
下面是配置多站点方案时的已知问题：  
  
-   **同一 IPv4 子网中存在多个入口点**。 在同一 IPv4 子网中添加多个入口点将导致 IP 地址冲突，并且不会按预期方式配置入口点的 DNS64 地址。 如果尚未将 IPv6 部署到企业网络上的服务器的内部接口上，则会出现此问题。 若要避免此问题，请在当前和未来的所有远程访问服务器上运行以下 Windows PowerShell 命令：  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   如果为 DirectAccess 客户端指定的公共地址连接到远程访问服务器时，NRPT 中包含一个后缀，则 DirectAccess 可能无法按预期工作。 确保 NRPT 对公共名称具有例外。 在多站点部署中，应为所有入口点的公用名称添加例外。 请注意，如果启用了强制隧道，则会自动添加这些例外。 如果禁用强制隧道，则会删除它们。  
  
-   使用 Windows PowerShell cmdlet **DAMultiSite**时，WhatIf 和 Confirm 参数不起作用，将禁用 Multisite，并删除 Windows 7 gpo。  
  
-   在多站点部署中使用 DCA 的 Windows 7 客户端升级到 Windows 8 时，网络连接助手将无法工作。 在客户端升级之前，可以通过使用以下 Windows PowerShell cmdlet 修改 Windows 7 Gpo 来解决此问题：  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    如果客户端已经升级，则将客户端计算机移动到 Windows 8 安全组。  
  
-   使用 Windows PowerShell cmdlet **set-daentrypointdc**修改域控制器设置时，如果指定的 ComputerName 参数是远程访问服务器，而不是添加到多站点部署中的最后一个入口点，则会出现警告将显示，指示在下一次策略刷新之前，不会更新指定的服务器。 在**远程访问管理控制台**的**仪表板**中，可以使用 "**配置状态**" 查看未更新的实际服务器。 但这不会导致任何功能问题，但是，你可以在未更新的服务器上运行**gpupdate/force** ，以立即更新配置状态。  
  
-   当多站点部署在仅支持 IPv4 的公司网络中时，更改内部网络 IPv6 前缀还会更改 DNS64 地址，但不会更新允许 DNS 查询到 DNS64 服务的防火墙规则上的地址。 若要解决此问题，请在更改内部网络 IPv6 前缀后运行以下 Windows PowerShell 命令：  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   如果在存在现有 ISATAP 基础结构时部署了 DirectAccess，则在删除作为 ISATAP 主机的入口点时，将从 NRPT 中所有 DNS 后缀的 DNS 服务器地址中删除 DNS64 服务的 IPv6 地址。  
  
    若要解决此问题，请在 "**基础结构服务器设置**向导" 的 " **dns** " 页上，使用正确的 dns 服务器地址删除已修改的 DNS 后缀并再次添加它们，方法是在 dns 服务器地址上单击 "**检测**"对话框。  
  


