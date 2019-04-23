---
title: 网络控制器
description: 本主题概述了 Windows Server 2016 中的网络控制器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ace628c6ae9802c0c65d360aedfac8c80ac5537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875678"
---
# <a name="network-controller"></a>网络控制器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

新 Windows Server 2016 中，在网络控制器提供集中的可编程点，可用于管理、 配置、 监视和故障排除你的数据中心中的虚拟和物理网络基础结构自动。 

使用网络控制器可以自动配置网络基础结构，而无需手动执行网络设备和服务的配置。

> [!NOTE]
> 除了本主题提供了以下网络控制器文档。
> - [网络控制器的高可用性](network-controller-high-availability.md)
> - [安装和部署网络控制器的准备要求](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [部署网络控制器使用 Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [安装网络控制器服务器角色使用服务器管理器](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [网络控制器的部署后步骤](post-deploy-steps-nc.md)
> - [网络控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>网络控制器概述

网络控制器是高度可用且可伸缩的服务器角色，并提供一个应用程序编程接口\(API\) ，使网络控制器与网络进行通信，您可以第二个 API与网络控制器进行通信。

你可以部署网络控制器在域和非域环境中。 在域环境中，网络控制器进行身份验证的用户和网络设备使用 Kerberos;在非域环境，必须部署进行身份验证的证书。

>[!IMPORTANT]
>不要部署物理主机上的网络控制器服务器角色。 若要部署网络控制器，必须安装网络控制器服务器角色上的 HYPER-V 虚拟机\(VM\)安装在 HYPER-V 主机上。 在三个不同超上的 Vm 上安装网络控制器后\-V 主机，必须启用超\-V 软件定义的网络的主机\(SDN\)通过添加到网络控制器使用的主机Windows PowerShell 命令**新建 NetworkControllerServer**。 这样，要启用 SDN 软件负载均衡器函数。 有关详细信息，请参阅[新建 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

网络控制器使用 Southbound API 与网络设备、服务和组件进行通信。 借助 Southbound API，网络控制器可以发现网络设备、检测服务配置并收集用户所需的所有网络信息。 此外，Southbound API 还为网络控制器提供了一种将信息发送到网络基础结构的方式，例如用户已做出的配置更改。

网络控制器 Northbound API 使你能够从网络控制器收集网络信息并将其用于监视和配置网络。

网络控制器 Northbound API 允许你配置、 监视、 故障排除，以及部署在网络上的新设备通过 Windows PowerShell、 表述性状态转移\(REST\) API 或管理应用程序使用图形用户界面，如 System Center Virtual Machine Manager。

>[!NOTE]
>网络控制器 Northbound API 作为 REST 接口而实现。

可以通过使用管理应用程序，如 System Center Virtual Machine Manager 中管理数据中心网络使用网络控制器\(SCVMM\)，和 System Center Operations Manager \(SCOM\)，由于网络控制器允许你配置，监视程序，并对其控制下的网络基础结构进行故障排除。

通过使用 Windows PowerShell、REST API 或管理应用程序，你可以使用网络控制器来管理以下物理和虚拟网络基础结构：

- Hyper-V VM 和虚拟交换机

- 数据中心防火墙

- 远程访问服务\(RAS\)多租户网关、 虚拟网关和网关池

- 软件负载均衡器

在下图中，管理员使用一种可与网络控制器直接交互的管理工具。 网络控制器提供了有关网络基础结构，包括虚拟和物理基础结构，为管理工具的信息，并使根据管理员的操作时使用该工具的配置更改。  

![网络控制器概述](../../../media/Network-Controller/NetController_overview.png)  

如果要在测试实验室环境中部署网络控制器，可以在 HYPER-V 虚拟机上运行网络控制器服务器角色\(VM\)安装在 HYPER-V 主机上。

为了在较大的数据中心的高可用性，可以通过使用安装在三个或多个 HYPER-V 主机的三个 Vm 部署群集。 有关详细信息，请参阅[网络控制器的高可用性](network-controller-high-availability.md)。

## <a name="bkmk_features"></a>网络控制器功能

以下网络控制器功能可用于配置和管理虚拟和物理网络设备和服务。  
  
-   [防火墙管理](#bkmk_firewall)  
  
-   [软件负载均衡器管理](#bkmk_slb)  
  
-   [虚拟网络管理](#bkmk_virtual)  
  
-   [RAS 网关管理](#bkmk_gateway)

>[!IMPORTANT]
>网络控制器备份和还原不是 Windows Server 2016 中当前可用的。
  
### <a name="bkmk_firewall"></a>防火墙管理

借助此网络控制器功能，你能够为你的工作负荷 VM 配置和管理同时针对数据中心中的横向和纵向网络流量的允许/拒绝防火墙访问控制规则。 工作负荷 VM 的 vSwitch 端口将查明防火墙规则，以便将其分布到数据中心中的工作负荷上。 通过使用 Northbound API，你可以从工作负荷 VM 同时为传入和传出流量定义防火墙规则。 还可以配置每条防火墙规则，以记录该规则允许或拒绝的流量。  

有关详细信息，请参阅[数据中心防火墙概述](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。

### <a name="bkmk_slb"></a>软件负载均衡器管理

此网络控制器功能可使你允许多个服务器承载相同的工作负荷，从而提供高可用性和可扩展性。  
  
有关详细信息，请参阅[软件负载平衡&#40;SLB&#41;用于 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
### <a name="bkmk_virtual"></a>虚拟网络管理

此网络控制器功能允许你部署和配置 Hyper-V 网络虚拟化（包括个别 VM 上的 Hyper-V 虚拟交换机和虚拟网络适配器），并可用于存储和分发虚拟网络策略。

网络控制器同时支持网络虚拟化通用路由封装 (NVGRE) 和虚拟可扩展局域网 (VXLAN)。

### <a name="bkmk_gateway"></a>RAS 网关管理

此网络控制器功能，可部署、 配置和管理池成员的 RAS 网关，为你的租户提供网关服务的虚拟机 (Vm)。 网络控制器可以自动部署具有以下网关功能运行 RAS 网关的 Vm:

> [!NOTE]
> 在 System Center Virtual Machine Manager 中，RAS 网关名为 Windows Server 网关。

- 从群集中添加和删除网关 VM，并指定所需的备份级别。

- 使用 IPsec 在远程租户网络和你的数据中心之间进行站点到站点的虚拟专用网络 (VPN) 网关连接。

- 使用通用路由封装 (GRE) 在远程租户网络和你的数据中心之间进行站点到站点的 VPN 网关连接。

- 第 3 层转发功能。

- 边界网关协议 (BGP) 路由，以便您可以管理的租户的 VM 网络与其远程站点之间的网络流量的路由。

网络控制器可以置于单独的网关的租户不同的连接。 可以使用适用于所有网关连接的单个公共 IP，也可以具有不同的公共 Ip 子集的连接。 网络控制器中记录所有网关配置和状态更改，可以用于审核和故障排除。

有关 BGP 的详细信息，请参阅[边界网关协议&#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)。

RAS 网关的详细信息，请参阅[用于 SDN 的 RAS 网关](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="network-controller-deployment-options"></a>网络控制器部署选项

若要使用 System Center Virtual Machine Manager 部署网络控制器\(VMM\)，请参阅[设置 SDN 网络控制器在 VMM 构造中](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要部署网络控制器使用脚本，请参阅[部署软件定义网络基础结构使用的脚本](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要部署网络控制器使用 Windows PowerShell，请参阅[部署网络控制器使用 Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
