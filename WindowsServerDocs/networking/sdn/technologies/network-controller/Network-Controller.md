---
title: 网络控制器
description: 本主题概述了 Windows Server 2016 中的网络控制器。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 13f535b9a91f26b30600b637b46817cfa33ccd7b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355646"
---
# <a name="network-controller"></a>网络控制器

>适用于：Windows Server（半年频道）、Windows Server 2016

Windows Server 2016 中的新增功能，网络控制器提供集中的可编程点，可用于管理、配置、监视和排查数据中心中的虚拟和物理网络基础结构。 

使用网络控制器可以自动配置网络基础结构，而无需手动执行网络设备和服务的配置。

> [!NOTE]
> 除了本主题之外，还提供了以下网络控制器文档。
> - [网络控制器高可用性](network-controller-high-availability.md)
> - [部署网络控制器的安装和准备要求](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [使用 Windows PowerShell 部署网络控制器](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [使用服务器管理器安装网络控制器服务器角色](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [网络控制器的部署后步骤](post-deploy-steps-nc.md)
> - [网络控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>网络控制器概述

网络控制器是高度可用且可缩放的服务器角色，它提供一个应用程序编程接口 \(API @ no__t-1，使网络控制器可以与网络通信，另一个 API 用于与网络控制器。

你可以在域和非域环境中部署网络控制器。 在域环境中，网络控制器使用 Kerberos 对用户和网络设备进行身份验证;在非域环境中，你必须部署用于身份验证的证书。

>[!IMPORTANT]
>不要将网络控制器服务器角色部署到物理主机上。 若要部署网络控制器，必须在安装在 Hyper-v 主机上的 Hyper-v 虚拟机 \(VM @ no__t-1 上安装网络控制器服务器角色。 在三个不同的 no__t 主机上的虚拟机上安装了网络控制器之后，必须通过使用 Windows PowerShell 将主机添加到网络控制器，为软件定义的网络启用超级 @ no__t-1V 主机 \(SDN @ no__t**NetworkControllerServer**命令。 这样做会使 SDN 软件负载均衡器正常工作。 有关详细信息，请参阅[NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

网络控制器使用 Southbound API 与网络设备、服务和组件进行通信。 借助 Southbound API，网络控制器可以发现网络设备、检测服务配置并收集用户所需的所有网络信息。 此外，Southbound API 还为网络控制器提供了一种将信息发送到网络基础结构的方式，例如用户已做出的配置更改。

网络控制器 Northbound API 使你能够从网络控制器收集网络信息并将其用于监视和配置网络。

网络控制器 Northbound API 允许使用 Windows PowerShell、具象状态传输 \(REST @ no__t API 或带有图形的管理应用程序，在网络上配置、监视、故障排除和部署新设备用户界面，如 System Center Virtual Machine Manager。

>[!NOTE]
>网络控制器 Northbound API 作为 REST 接口而实现。

你可以使用管理应用程序（如 System Center Virtual Machine Manager \(SCVMM @ no__t）管理数据中心网络，并 System Center Operations Manager \(SCOM @ no__t，因为网络控制器允许你配置、监视、计划和排查正在控制的网络基础结构。

通过使用 Windows PowerShell、REST API 或管理应用程序，你可以使用网络控制器来管理以下物理和虚拟网络基础结构：

- Hyper-V VM 和虚拟交换机

- 数据中心防火墙

- 远程访问服务 \(RAS @ no__t-1 多租户网关、虚拟网关和网关池

- 软件负载均衡器

在下图中，管理员使用一种可与网络控制器直接交互的管理工具。 网络控制器提供有关网络基础结构的信息，包括虚拟和物理基础结构和管理工具，并根据管理员在使用该工具时的操作进行配置更改。  

![网络控制器概述](../../../media/Network-Controller/NetController_overview.png)  

如果要在测试实验室环境中部署网络控制器，可以在 hyper-v 虚拟机上运行网络控制器服务器角色，\(VM @ no__t-1 安装在 Hyper-v 主机上。

若要在更大的数据中心内实现高可用性，可以使用安装在三个或更多 Hyper-v 主机上的三个 Vm 来部署群集。 有关详细信息，请参阅[网络控制器高可用性](network-controller-high-availability.md)。

## <a name="bkmk_features"></a>网络控制器功能

以下网络控制器功能可用于配置和管理虚拟和物理网络设备和服务。  
  
-   [防火墙管理](#bkmk_firewall)  
  
-   [软件负载平衡器管理](#bkmk_slb)  
  
-   [虚拟网络管理](#bkmk_virtual)  
  
-   [RAS 网关管理](#bkmk_gateway)

>[!IMPORTANT]
>网络控制器的备份和还原目前在 Windows Server 2016 中不可用。
  
### <a name="bkmk_firewall"></a>防火墙管理

借助此网络控制器功能，你能够为你的工作负荷 VM 配置和管理同时针对数据中心中的横向和纵向网络流量的允许/拒绝防火墙访问控制规则。 工作负荷 VM 的 vSwitch 端口将查明防火墙规则，以便将其分布到数据中心中的工作负荷上。 通过使用 Northbound API，你可以从工作负荷 VM 同时为传入和传出流量定义防火墙规则。 还可以配置每条防火墙规则，以记录该规则允许或拒绝的流量。  

有关详细信息，请参阅[数据中心防火墙概述](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。

### <a name="bkmk_slb"></a>软件负载平衡器管理

此网络控制器功能可使你允许多个服务器承载相同的工作负荷，从而提供高可用性和可扩展性。  
  
有关详细信息，请参阅用于[SDN &#40;的&#41;软件负载平衡 SLB](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
### <a name="bkmk_virtual"></a>虚拟网络管理

此网络控制器功能允许你部署和配置 Hyper-V 网络虚拟化（包括个别 VM 上的 Hyper-V 虚拟交换机和虚拟网络适配器），并可用于存储和分发虚拟网络策略。

网络控制器同时支持网络虚拟化通用路由封装 (NVGRE) 和虚拟可扩展局域网 (VXLAN)。

### <a name="bkmk_gateway"></a>RAS 网关管理

此网络控制器功能可用于部署、配置和管理作为 RAS 网关池成员的虚拟机（Vm），从而向租户提供网关服务。 网络控制器允许您通过以下网关功能自动部署运行 RAS 网关的 Vm：

> [!NOTE]
> 在 System Center Virtual Machine Manager 中，RAS 网关名为 "Windows Server 网关"。

- 从群集中添加和删除网关 VM，并指定所需的备份级别。

- 使用 IPsec 在远程租户网络和你的数据中心之间进行站点到站点的虚拟专用网络 (VPN) 网关连接。

- 使用通用路由封装 (GRE) 在远程租户网络和你的数据中心之间进行站点到站点的 VPN 网关连接。

- 第 3 层转发功能。

- 边界网关协议（BGP）路由，可用于管理租户的 VM 网络与其远程站点之间的网络流量路由。

网络控制器可以将不同的租户连接到不同的网关。 对于所有网关连接，可以使用单个公共 IP，或将不同的公共 IP 用于连接的子集。 网络控制器记录所有网关配置和状态更改，这些更改可用于审核和故障排除。

有关 BGP 的详细信息，请[参阅&#40;边界网关协议&#41;bgp](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)。

有关 RAS 网关的详细信息，请参阅[SDN 的 Ras 网关](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="network-controller-deployment-options"></a>网络控制器部署选项

若要使用 System Center Virtual Machine Manager \(VMM @ no__t 部署网络控制器，请参阅[在 VMM 构造中设置 SDN 网络控制器](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要使用脚本部署网络控制器，请参阅[使用脚本部署软件定义的网络基础结构](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要使用 Windows PowerShell 部署网络控制器，请参阅[使用 Windows Powershell 部署网络控制器](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
