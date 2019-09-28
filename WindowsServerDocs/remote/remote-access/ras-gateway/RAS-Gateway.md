---
title: RAS 网关
description: 本主题面向信息技术（IT）专业人员，提供有关 RAS 网关的概述信息，包括 Windows Server 2016 中的 RAS 网关部署模式和功能。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: pashort
author: shortpatti
ms.date: 05/23/2018
ms.openlocfilehash: ebf2cc840be771707f23d7976b670baae96c1343
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367490"
---
# <a name="ras-gateway"></a>RAS 网关

>适用于：Windows Server（半年频道）、Windows Server 2016

RAS 网关是一种软件路由器和网关，可在单租户模式或多租户模式下使用。  
  
- **单租户**模式允许任意规模的组织以外部或面向 Internet 的虚拟专用网络（VPN）和 DirectAccess 服务器的形式部署网关。 在单租户模式下，你可以在运行 Windows Server 2016 的物理服务器或虚拟机（VM）上部署 RAS 网关。  
  
- 多**租户模式**允许云服务提供商（csp）和企业使用 RAS 网关在虚拟网络与物理网络（包括 Internet）之间进行数据中心和云网络流量路由。 对于多租户模式，建议你在运行 Windows Server 2016 的 Vm 上部署 RAS 网关。  
  
> [!NOTE]  
> RAS 网关支持 IPv4 和 IPv6，包括 IPv4 和 IPv6 转发。 当你通过网络地址转换（NAT）配置 RAS 网关时，仅支持支持 NAT44。  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>谁会对 RAS 网关感兴趣？
  
如果你是系统管理员、网络架构师或其他 IT 专业人员，RAS 网关可能会在以下一种或多种情况下感兴趣：  
  
-   你要为某个组织设计 IT 基础结构或提供相应支持，而该组织正在使用或计划使用 Hyper-V 在虚拟网络中部署虚拟机 (VM)。  
  
-   你要为某个组织设计 IT 基础结构或提供相应支持，而该组织已部署或计划部署云技术。  
  
-   你想要在物理网络与虚拟网络之间提供全面的网络连接。  
  
-   你想让你的组织的客户能够通过 Internet 访问其虚拟网络。  
  
-   需要为组织的员工提供对组织网络的远程访问权限。  
  
-   想要跨 Internet 连接位于不同物理位置的办公室。  
 
本主题面向信息技术（IT）专业人员，提供有关 RAS 网关的概述信息，包括 RAS 网关部署模式和功能。 
  
本主题包含以下部分。  
  
  
-   [RAS 网关部署模式](#bkmk_modes)  
  
-   [群集 RAS 网关以实现高可用性](#bkmk_clustering)  
  
-   [RAS 网关功能](#bkmk_features)  
  
-   [RAS 网关部署方案](#bkmk_deploy)  
  
-   [RAS 网关管理工具](#bkmk_manage)  
  


  
## <a name="bkmk_modes"></a>RAS 网关部署模式  
RAS 网关包括以下部署模式：  
  
### <a name="single-tenant-mode"></a>单租户模式  
对于大多数组织而言，在单租户模式下使用 RAS 网关是典型配置。 在单租户模式下，你可以将 RAS 网关部署为边缘 VPN 服务器、边缘 DirectAccess 服务器或同时部署两者。 在此配置中，RAS 网关通过使用 VPN 或 DirectAccess 连接为远程员工提供与网络的连接。 此外，单租户模式允许跨 Internet 连接位于不同物理位置的办公室。  
  
### <a name="multitenant-mode"></a>多租户模式  
如果你的组织是拥有多个租户的 CSP 或企业，则可以在多租户模式下部署 RAS 网关，以提供与虚拟网络和物理网络之间的网络流量路由。  
  
多租户是云基础结构的功能，用于支持多个租户的虚拟机工作负荷，但将它们彼此隔离，而所有工作负荷在同一基础结构上运行。 单个租户的多个工作负载可以互连和接受远程管理，但这些系统不与其他租户的工作负载互连，其他租户也不能远程管理它们。  
  
例如，某家企业使用了许多不同的虚拟子网，每个虚拟子网专门为特定的部门（如研发或会计部门）提供服务。 另举一例，某家 CSP 有许多租户，他们的虚拟子网在同一物理数据中心内处于隔离状态。 在这两种情况下，RAS 网关可以将流量路由到每个租户，同时保持每个租户的专用隔离。 此功能使 RAS 网关具有多租户感知能力。  
  
虚拟网络是使用 Hyper-v 网络虚拟化创建的，Hyper-v 网络虚拟化是 Windows Server 2012 中引入的一种技术，在 Windows Server 2016 中进行了改进。 RAS 网关与 Hyper-v 网络虚拟化集成，在有许多不同的客户或租户（在同一数据中心内具有隔离的虚拟网络）的情况下，它可以有效地路由网络通信。  
  
Hyper-v 网络虚拟化提供了部署独立于基础物理网络的虚拟机（VM）网络的功能。 如果 VM 网络由一个或多个虚拟子网组成，则 IP 子网的确切物理位置与虚拟网络拓扑分离。 因此，你可以轻松地将本地子网移至云中，同时在云中保留现有 IP 地址和拓扑。 这种保留基础结构的能力使得现有服务能够继续工作，而不考虑子网的物理位置， 也就是说，Hyper-V 网络虚拟化可实现无缝混合云的建立。  
  
> [!NOTE]  
> Hyper-v 网络虚拟化是一种使用网络虚拟化通用路由封装（[NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)）的网络覆盖技术，它允许租户引入其自己的地址空间，并允许使用用于租户隔离的 Vlan。  
  
在 Windows Server 2016 中，RAS 网关可以在物理网络与 VM 网络资源之间路由网络流量，而不管资源位于何处。 可以使用 RAS 网关在位于同一物理位置或多个不同物理位置的物理网络与虚拟网络之间路由网络流量。  
  
例如，如果你的物理网络和虚拟网络位于同一个物理位置，则可以部署一个运行 Hyper-v 的计算机，该计算机配置有 RAS 网关 VM 来充当转发网关，并在虚拟和物理之间路由流量广域网.  
  
在另一个示例中，如果你的虚拟网络位于云中，则你的 CSP 可以部署 RAS 网关，以便你可以在 VPN 服务器与 CSP 的 RAS 网关之间创建虚拟专用网络（VPN）站点到站点连接;建立此链接后，可以通过 VPN 连接连接到云中的虚拟资源。  
  
有关详细信息，请参阅[RAS 网关高可用性](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_clustering"></a>群集 RAS 网关以实现高可用性  
RAS 网关部署在运行 Hyper-v 且配置了一个 VM 的专用计算机上。 然后，将 VM 配置为 RAS 网关。  
  
为实现网络资源的高可用性，可以使用两台运行 Hyper-v 的物理主机服务器（每个运行 Hyper-v 的物理主机服务器还运行配置为网关的虚拟机（VM））部署 RAS 网关进行故障转移。 然后，可将网关 VM 配置为群集，以便在出现网络中断和硬件故障时提供故障转移保护。  
  
例如，如果你的组织是具有私有云部署的企业，则你可能只需要两个 RAS 网关 Vm，其中每个 Vm 都安装在运行 Hyper-v 的另一台计算机上。 在这种情况下，会将 RAS 网关 Vm 添加到群集，以提供高可用性。  
  
在另一个示例中，如果你的组织是你的数据中心内包含200租户的云服务提供商（CSP），则可以使用八个 RAS 网关 Vm，每对群集 RAS 网关 Vm 为50租户提供路由服务。 在此方案中，两台运行 Hyper-v 的计算机都有四个配置为 RAS 网关的虚拟机。 然后配置四个 RAS 网关 VM 群集，每个群集都包含运行 Hyper-v 的每台计算机中的一个 VM。  
  
部署 RAS 网关时，运行 Hyper-v 以及配置为网关的 Vm 的主机服务器必须运行 Windows Server 2012 R2 或 Windows Server 2016。  
  
## <a name="bkmk_features"></a>RAS 网关功能  
RAS 网关包含以下功能。  
  
-   **站点到站点 VPN**。 此 RAS 网关功能允许你使用站点到站点 VPN 连接在 Internet 上的不同物理位置连接两个网络。 如果你有一个总部和多个分支机构，则可以在每个位置部署一个边缘 RAS 网关，并创建站点到站点连接，以提供位置之间的网络流量。 对于在数据中心托管许多租户的 Csp，RAS 网关提供多租户网关解决方案，使租户能够通过站点到站点 VPN 连接访问和管理其资源，并允许网络流量在数据中心及其物理网络中的虚拟资源。  
  
-   **点到站点 VPN**。 此 RAS 网关功能允许组织员工或管理员从远程位置连接到组织的网络。 对于 RAS 网关的单租户部署，远程员工可以使用 VPN 连接来连接到你的组织网络。 通过此连接，用户可以使用内部网络资源，如 intranet 网站和文件服务器。 对于多租户部署，租户网络管理员可以使用点到站点 VPN 连接来访问 CSP 数据中心的虚拟网络资源。  
  
-   **动态路由边界网关协议（BGP）** 。 BGP 可减少对在路由器上进行手动路由配置的需要，因为它是一种动态路由协议，可自动了解使用站点到站点 VPN 连接进行连接的站点之间的路由。 如果你的组织有多个使用启用了 BGP 的路由器（如 RAS 网关）连接的站点，则在发生网络中断或故障时，BGP 允许路由器自动计算并使用有效的路由。 有关详细信息，请参阅[RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  
-   **网络地址转换（NAT）** 。 通过网络地址转换（NAT），可以通过单个公共 IP 地址的单一界面与公共 Internet 建立连接。 专用网络上的计算机使用专用的不可路由地址。 NAT 将专用地址映射到公共地址。 此 RAS 网关功能允许具有单租户部署的组织员工使用网关后访问 Internet 资源。 对于 Csp，此功能允许租户 Vm 上运行的应用程序访问 Internet。 例如，配置为 Web 服务器的租户 VM 可以联系外部财务资源来处理信用卡交易。  

  
## <a name="bkmk_deploy"></a>RAS 网关部署方案  
以下是 RAS 网关的推荐部署方案。  
  
-   **企业边缘-单租户部署**。 使用单租户企业部署，你可以通过站点到站点 VPN 功能将一个物理位置连接到多个其他物理位置，并边界网关协议（BGP）允许你使用动态路由。 你还可以使用点到站点 VPN 连接和 DirectAccess 连接向远程员工提供对你的组织网络的访问权限。 （DirectAccess 连接始终处于启用状态，并且还提供了可轻松管理使用 DirectAccess 连接的计算机的优势，因为这些计算机在连接和连接 Internet 时都处于连接状态。）你还可以通过 NAT 配置单租户企业 RAS 网关，使你的 Intranet 上的计算机可以轻松地与 Internet 进行通信。  
  
-   **云服务提供商边缘-多租户部署**。 Csp 的 RAS 网关多租户部署可让你为你的租户提供企业边缘单租户部署提供的所有功能。 你的数据中心内的租户虚拟网络与 Internet 上的租户网络位置之间的站点到站点 VPN 连接意味着租户始终能够无缝地访问其云资源。 租户的点到站点 VPN 访问意味着租户管理员始终可以连接到你的数据中心内的虚拟网络，以管理其资源。 BGP 提供动态路由，并使租户连接到其资产，即使在 Internet 或其他地方出现网络问题时也是如此。 和 NAT 允许租户 Vm 连接到 Internet 上的资源，例如信用卡处理资源。  
  
## <a name="bkmk_manage"></a>RAS 网关管理工具  
以下是 RAS 网关的管理工具。  
  
-   在 Windows Server 2016 中，若要部署 RAS 网关路由器，必须使用 Windows PowerShell 命令。 有关详细信息，请参阅适用于 Windows Server 2016 和 Windows 10 的[远程访问 cmdlet](https://docs.microsoft.com/powershell/module/remoteaccess) 。  
  
-   在 System Center 2012 R2 Virtual Machine Manager （VMM）中，RAS 网关名为 "Windows Server 网关"。 VMM 软件界面中提供一组有限的边界网关协议（BGP）配置选项，包括**本地 BGP IP 地址**和**自治系统编号（ASN）** 、 **BGP 对等 ip 地址列表**和**ASN 值**. 但是，你可以使用远程访问 Windows PowerShell BGP 命令来配置 Windows Server 网关的所有其他功能。 有关详细信息，请参阅[Virtual Machine Manager （VMM）](https://technet.microsoft.com/system-center-docs/vmm/vmm)和 windows Server 2016 和 windows 10 的[远程访问 cmdlet](https://technet.microsoft.com/library/hh918399.aspx) 。  
  
## <a name="related-topics"></a>相关主题
- [RAS 网关高可用性](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Windows Server 中的 GRE 隧道](gre-tunneling-windows-server.md)
- [RAS 网关 GRE 隧道吞吐量和性能](RAS-Gateway-GRE-Perf.md)
