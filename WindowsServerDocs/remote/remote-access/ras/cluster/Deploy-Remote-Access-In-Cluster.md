---
title: 在群集中部署远程访问
description: 本主题是在 Windows Server 2016 的群集中部署远程访问指南的一部分。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e4108eee0c62ae4d4db31560b31a6f90751c6b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404642"
---
# <a name="deploy-remote-access-in-a-cluster"></a>在群集中部署远程访问

>适用于：Windows Server（半年频道）、Windows Server 2016

Windows Server 2016 和 Windows Server 2012 将 DirectAccess 和远程访问服务 \(RAS @ no__t-1 VPN 合并到单个远程访问角色中。 你可以在多个企业方案中部署远程访问。 本概述介绍在使用 Windows 网络负载平衡的群集负载中部署多台远程访问服务器的企业方案 \(NLB @ no__t-1 或外部负载均衡器 \(ELB @ no__t，例如 F5Big @ no__t-4IP。  

## <a name="BKMK_OVER"></a>方案描述  
群集部署将多台远程访问服务器收集到一个单元中，后者随后将充当使用外部虚拟 IP 通过 DirectAccess 或 VPN 连接到内部公司网络的远程客户端计算机的单一联系点 @no__0VIP @ no__t-1 远程访问群集的地址。  到群集的流量使用 Windows NLB 或外部负载均衡器进行负载均衡，\(such 作为 F5 Big @ no__t-1IP @ no__t-2。  

## <a name="prerequisites"></a>先决条件  
在开始部署此方案之前，请查看此列表以了解重要要求：  

-   默认通过 Windows NLB 进行负载平衡。  

-   支持外部负载平衡器。  

-   NLB 的默认和建议模式为单播模式。  

-   不支持在 DirectAccess 管理控制台或 PowerShell cmdlet 之外更改策略。  

-   使用 NLB 或外部负载平衡器时，不能将 IPHTTPS 前缀更改为 \/59 以外的任何内容。  

-   负载平衡节点必须位于同一 IPv4 子网中。  

-   在 ELB 部署中，如果需要 "管理"，则 DirectAccess 客户端不能使用 @ no__t-0Teredo。 仅 IPHTTPS 可用于 end @ no__t-0to @ no__t-1end 通信。  

-   确保已安装所有已知的 NLB @ no__t-0ELB 修补程序。  

-   企业网络中的 ISATAP 不受支持。 如果你使用的是 ISATAP，应该将其删除并使用本机 IPv6。  

## <a name="in-this-scenario"></a>本方案内容  
群集部署方案包含许多步骤：  

1.  [使用高级选项部署 ALWAYSON VPN 服务器](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md)。 在设置群集部署之前，必须先部署具有高级设置的单个远程访问服务器。  

2.  [规划远程访问群集部署](plan/Plan-a-Remote-Access-Cluster-Deployment.md)。 若要从单个服务器部署生成群集，需要执行多个其他步骤，包括为群集部署准备证书。  

3.  [配置远程访问群集](configure/Configure-a-Remote-Access-Cluster.md)。 这包括多个配置步骤，包括为 Windows NLB 或外部负载均衡器准备单一服务器、准备其他服务器以加入群集以及启用负载平衡。  

## <a name="BKMK_APP"></a>实用应用程序  
将多台服务器收集到服务器群集中可实现以下优势：  

-   可扩展性。 单个远程访问服务器提供了有限的服务器可靠性和可伸缩性能。 将两台或更多服务器资源组合到单一群集中，可提升吞吐量以及增加用户数目。  

-   高可用性。 群集为 always @ no__t-0on access 提供高可用性。 如果群集中的服务器出现故障，远程用户可继续通过群集中的其他服务器访问企业网络。 群集中的所有服务器都具有一组相同的群集虚拟 IP \(VIP @ no__t 地址，同时仍保留每个服务器的唯一专用 IP 地址。  

-   缓动 @ no__t-0of @ no__t-1management。 群集允许将多个服务器作为单个实体进行管理。 可在各个群集服务器之间轻松共享设置。 可从群集中的任何服务器管理远程访问设置，或使用远程服务器管理工具 \(RSAT @ no__t-1 远程访问远程访问设置。 此外，可从单个远程访问管理控制台监视整个群集。  

## <a name="BKMK_NEW"></a>此方案中包含的角色和功能  
下表列出了本方案所需的角色和功能：  

|Role @ no__t-0feature|如何支持本方案|  
|---------|-----------------|  
|远程访问角色|该角色可使用服务器管理器控制台加以安装和卸载。 它包括 DirectAccess （以前是 Windows Server 2008 R2 中的一项功能）以及路由和远程访问服务 \(RRAS @ no__t-1，它以前是网络策略和访问服务 \(NPAS @ no__t 中的角色服务服务器角色。 远程访问角色由以下两个组件组成：<br /><br />-Always On VPN 和路由和远程访问服务 \(RRAS @ no__t VPN-DirectAccess 和 VPN 在远程访问管理控制台中一起进行管理。<br />-RRAS 路由-RRAS 路由功能在旧版路由和远程访问控制台中进行管理。<br /><br />依赖关系如下所示：<br /><br />-Internet Information Services \(IIS @ no__t-1 Web 服务器-配置网络位置服务器和默认 Web 探测需要此功能。<br />-Windows 内部数据库-用于远程访问服务器上的本地记帐。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-在安装远程访问角色时，它默认安装在远程访问服务器上，并支持远程管理控制台用户界面。<br />-可选择将它安装在不运行远程访问服务器角色的服务器上。 在这种情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />-远程访问 GUI 和命令行工具<br />-适用于 Windows PowerShell 的远程访问模块<br /><br />依赖项包括：<br /><br />-组策略管理控制台<br />-RAS 连接管理器管理工具包 \(CMAK @ no__t-1<br />-Windows PowerShell 3。0<br />-图形管理工具和基础结构|  
|网络负载平衡|此功能通过 Windows NLB 提供群集中的负载平衡。|  

## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  

-   至少两台满足 Windows Server 2012 硬件要求的计算机。  

-   对于外部负载平衡器方案，需要专用硬件 \( i BigIP @ no__t。  

-   若要测试该方案，必须至少有一台运行 Windows 10 的计算机配置为 Always On VPN 客户端。   

## <a name="BKMK_SOFT"></a>软件要求  
存在许多对本方案的要求。  

-   对单台服务器部署的软件要求。 有关详细信息，请参阅[使用高级设置部署单个 DirectAccess 服务器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 单个远程访问）。  

-   除单台服务器的软件要求外，还存在许多群集 @ no__t-0specific 要求：  

    -   在每个群集服务器上，IP @ no__t-0HTTPS 证书使用者名称必须与 ConnectTo 地址匹配。 群集部署支持在群集服务器上混合使用通配符和非 @ no__t-0wildcard 证书。  

    -   如果远程访问服务器上安装了网络位置服务器，则每台群集服务器上的网络位置服务器证书必须拥有相同的使用者名称。 此外，网络位置服务器证书的名称不能与 DirectAccess 部署中任何服务器的名称相同。  

    -   必须使用与颁发给单一服务器的证书相同的方法颁发 IP @ no__t-0HTTPS 和网络位置服务器证书。 例如，如果单一服务器使用公共证书颁发机构 \(CA @ no__t-1，则群集中的所有服务器都必须拥有公共 CA 颁发的证书。 或者，如果单台服务器使用 IP @ no__t-1HTTPS 的自 @ no__t-0signed 证书，则群集中的所有服务器都必须进行同样的操作。  

    -   分配给服务器群集上的 DirectAccess 客户端计算机的 IPv6 前缀必须是 59 位。 如果启用了 VPN，则 VPN 前缀必须是 59 位。  

## <a name="KnownIssues"></a>已知问题  
下面是配置群集方案时的已知问题：  

-   使用单个网络适配器在 IPv4 @ no__t-0only 部署中配置 DirectAccess 之后，在网络适配器上自动配置包含 "：3333：：" @no__t 的默认 DNS64 \(the IPv6 地址之后，尝试启用通过远程访问管理控制台加载 @ no__t-3balancing 会导致用户提供 IPv6 DIP 的提示。 如果提供的是 IPv6 DIP，则单击“提交”后配置将失败，并出现以下错误：参数不正确。  

    解决此问题：  

    1.  从 [备份和还原远程访问配置](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6)中下载备份和还原脚本。  

    2.  使用下载的脚本 Backup @ no__t-0RemoteAccess 备份远程访问 Gpo  

    3.  尝试启用负载平衡，直至达到失败的步骤。 在 "启用负载平衡" 对话框中，展开详细信息区域，在详细信息区域中右 @ no__t-0click，然后单击 "**复制脚本**"。  

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

    8.  如果该 cmdlet 在运行时失败 \(not，因为输入值 @ no__t 不正确，请运行命令 Restore @ no__t-2RemoteAccess，并按照说明进行操作，以确保保留原始配置的完整性。  

    9. 现在可以重新打开远程访问管理控制台。  
