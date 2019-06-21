---
title: 在群集中部署远程访问
description: 本主题是指南的在 Windows Server 2016 中的群集中部署远程访问的一部分。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 853788f20c452391c802f0681fa23978b4892c6a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281230"
---
# <a name="deploy-remote-access-in-a-cluster"></a>在群集中部署远程访问

>适用于：Windows 服务器 （半年频道），Windows Server 2016

Windows Server 2016 和 Windows Server 2012 将 DirectAccess 和远程访问服务结合起来\(RAS\) VPN 到单个远程访问角色。 你可以部署许多个企业方案中的远程访问。 此概述介绍的企业方案的部署的群集负载中的多台远程访问服务器与 Windows 网络负载平衡保持平衡\(NLB\)或使用外部负载均衡器\(ELB\)，如 F5 大\-IP。  

## <a name="BKMK_OVER"></a>应用场景说明  
群集部署将多个远程访问服务器汇总为单个单元，然后充当单点联系远程客户端计算机通过 DirectAccess 或 VPN 连接到企业内部网络使用的外部虚拟 IP \(VIP\)远程访问群集的地址。  到群集的流量进行负载平衡使用 Windows NLB 或外部负载均衡器\(如 F5 大\-IP\)。  

## <a name="prerequisites"></a>系统必备  
在开始部署此方案之前，请查看此列表以了解重要要求：  

-   默认通过 Windows NLB 进行负载平衡。  

-   支持外部负载平衡器。  

-   NLB 的默认和建议模式为单播模式。  

-   不支持在 DirectAccess 管理控制台或 PowerShell cmdlet 之外更改策略。  

-   当使用 NLB 或外部负载均衡器时，IPHTTPS 前缀不能更改为任何内容而不\/59。  

-   负载平衡节点必须位于同一 IPv4 子网中。  

-   在 ELB 部署中，如果管理需要向外，那么 DirectAccess 客户端不能使用&nbsp;Teredo。 仅 IPHTTPS 可用于结束\-到\-结束的通信。  

-   确保所有已知的 NLB\/ELB 修补程序安装。  

-   企业网络中的 ISATAP 不受支持。 如果你使用的是 ISATAP，应该将其删除并使用本机 IPv6。  

## <a name="in-this-scenario"></a>本方案内容  
群集部署方案包含许多步骤：  

1.  [部署使用高级选项的 VPN 服务器上始终](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md)。 设置群集部署之前，必须部署单个远程访问服务器使用高级设置。  

2.  [规划远程访问群集部署](plan/Plan-a-Remote-Access-Cluster-Deployment.md)。 生成多个其他的单服务器部署中的群集的步骤是必需的这包括为群集部署准备证书。  

3.  [配置远程访问群集](configure/Configure-a-Remote-Access-Cluster.md)。 这包括多个配置步骤，包括让单台服务器为 Windows NLB 或外部负载均衡器、 准备加入群集，其他服务器以及启用负载平衡。  

## <a name="BKMK_APP"></a>实际应用程序  
将多台服务器收集到服务器群集中可实现以下优势：  

-   可扩展性。 单台远程访问服务器提供有限的服务器可靠性和可缩放的性能。 将两台或更多服务器资源组合到单一群集中，可提升吞吐量以及增加用户数目。  

-   高可用性。 群集为始终提供高可用性\-的访问权限。 如果群集中的服务器出现故障，远程用户可继续通过群集中的其他服务器访问企业网络。 在群集中的所有服务器都具有一组相同的群集虚拟 IP \(VIP\)地址，同时仍保持唯一的专门为每个服务器的 IP 地址。  

-   轻松\-的\-管理。 群集允许多个服务器作为单个实体进行管理。 可在各个群集服务器之间轻松共享设置。 可以从任何群集，或使用远程服务器管理工具远程服务器管理远程访问设置\(RSAT\)。 此外，可从单个远程访问管理控制台监视整个群集。  

## <a name="BKMK_NEW"></a>在此方案中包括角色和功能  
下表列出了本方案所需的角色和功能：  

|角色\/功能|如何支持本方案|  
|---------|-----------------|  
|远程访问角色|该角色可使用服务器管理器控制台加以安装和卸载。 它包括 DirectAccess，以前是 Windows Server 2008 R2，以及路由和远程访问服务中的功能\(RRAS\)，以前是网络策略下的角色服务和访问服务\(NPAS\)服务器角色。 远程访问角色由以下两个组件组成：<br /><br />-Always On VPN 以及路由和远程访问服务\(RRAS\) VPN DirectAccess 和 VPN 由远程访问管理控制台中一起管理。<br />的在传统路由和远程访问控制台中管理 RRAS 路由 — RRAS 路由功能。<br /><br />依赖关系如下所示：<br /><br />-Internet Information Services \(IIS\) Web 服务器-此功能，才能配置网络位置服务器和默认 web 探测。<br />的远程访问服务器上的本地帐户的 Windows 内部 Database-Used。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-如果远程访问角色安装，并且支持远程管理控制台用户界面它是默认情况下，远程访问服务器上安装。<br />-它可以在不运行远程访问服务器角色的服务器上选择性地安装。 在这种情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />-远程访问 GUI 和命令行工具<br />的 Windows PowerShell 远程访问模块<br /><br />依赖项包括：<br /><br />组策略管理控制台<br />-RAS 连接管理器管理工具包\(CMAK\)<br />-   Windows PowerShell 3.0<br />图形管理工具和基础结构|  
|网络负载平衡|此功能通过 Windows NLB 提供群集中的负载平衡。|  

## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  

-   至少两台计算机满足 Windows Server 2012 的硬件要求。  

-   对于外部负载均衡器方案中，专用的硬件是所需\(即 F5 BigIP\)。  

-   为测试该方案，必须具有至少一台运行 Windows 10 配置为 Always On VPN 客户端计算机。   

## <a name="BKMK_SOFT"></a>软件要求  
存在许多对本方案的要求。  

-   对单台服务器部署的软件要求。 有关详细信息请参阅[部署单个 DirectAccess 服务器使用高级设置](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 单个远程访问）。  

-   除了单个服务器的软件要求有大量的群集\-特定要求：  

    -   在每个群集服务器的 IP\-HTTPS 证书使用者名称必须与 ConnectTo 地址匹配。 群集部署支持混合使用通配符和非\-群集服务器上的通配符证书。  

    -   如果远程访问服务器上安装了网络位置服务器，则每台群集服务器上的网络位置服务器证书必须拥有相同的使用者名称。 此外，网络位置服务器证书的名称不能与 DirectAccess 部署中任何服务器的名称相同。  

    -   IP\-必须使用相同的方法与颁发证书提供给单个服务器发出 HTTPS 和网络位置服务器证书。 例如，如果单台服务器使用公共证书颁发机构\(CA\)则在群集中的所有服务器必须都具有公共 CA 颁发的证书。 或如果单台服务器使用自助\-签名证书的 IP\-HTTPS，然后在群集中的所有服务器必须同样执行。  

    -   分配给服务器群集上的 DirectAccess 客户端计算机的 IPv6 前缀必须是 59 位。 如果启用了 VPN，则 VPN 前缀必须是 59 位。  

## <a name="KnownIssues"></a>已知的问题  
下面是配置群集方案时的已知问题：  

-   之后在 IPv4 中配置 DirectAccess\-部署使用单个网络适配器及默认的 DNS64 \(IPv6 地址，其中包含": 3333::"\)网络适配器上自动配置尝试启用负载\-平衡通过远程访问管理控制台将导致为用户提供 IPv6 DIP 的提示。 如果提供的是 IPv6 DIP，则单击“提交”  后配置将失败，并出现以下错误：参数不正确。  

    解决此问题：  

    1.  从 [备份和还原远程访问配置](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6)中下载备份和还原脚本。  

    2.  备份远程访问 Gpo 使用下载脚本备份\-RemoteAccess.ps1  

    3.  尝试启用负载平衡，直至达到失败的步骤。 在启用负载平衡对话框中，展开详细信息区域，右\-在详细信息区域中，单击，然后单击**复制脚本**。  

    4.  打开记事本，然后粘贴剪贴板的内容。 例如：  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    5.  关闭处于打开状态的所有“远程访问”对话框，并关闭远程访问管理控制台。  

    6.  编辑所粘贴的文本并删除 IPv6 地址。 例如：  

        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  

    7.  在提升的 PowerShell 窗口中，运行之前步骤的命令。  

    8.  如果在运行时，该 cmdlet 失败\(不是由不正确的输入值引起\)，运行命令还原\-RemoteAccess.ps1 并按照说明进行操作以确保维持原始配置的完整性.  

    9. 现在可以重新打开远程访问管理控制台。  
