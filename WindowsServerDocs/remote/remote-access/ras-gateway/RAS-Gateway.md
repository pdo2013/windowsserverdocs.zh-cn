---
title: RAS 网关
description: 适用于信息技术 (IT) 专业人员，本主题提供有关 RAS 网关，包括 Windows Server 2016 中的 RAS 网关部署模式和功能的概述信息。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: pashort
author: shortpatti
ms.date: 05/23/2018
ms.openlocfilehash: 8fc1c97d7c2a8694e56cc36b5501a82081b3db23
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812338"
---
# <a name="ras-gateway"></a>RAS 网关

>适用于：Windows 服务器 （半年频道），Windows Server 2016

RAS 网关是软件路由器和网关，可以在单租户模式下或多租户模式下使用。  
  
- **单租户**模式允许为外部，或面向 Internet 的边缘虚拟专用网络 (VPN) 和 DirectAccess 服务器部署网关任何规模的组织。 在单租户模式下，你可以在物理服务器或运行 Windows Server 2016 的虚拟机 (VM) 上部署 RAS 网关。  
  
- **多租户模式**允许云服务提供商 (Csp) 和企业能够使用 RAS 网关来启用数据中心和云网络虚拟和物理网络，包括 Internet 之间路由流量。 对于多租户模式，建议你部署运行 Windows Server 2016 的 Vm 上的 RAS 网关。  
  
> [!NOTE]  
> RAS 网关支持 IPv4 和 IPv6，包括 IPv4 和 IPv6 转发。 在使用网络地址转换 (NAT) 配置 RAS 网关，只支持 nat44。  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>谁会对感兴趣 RAS 网关？
  
如果你是系统管理员、 网络架构师或其他 IT 专业人员，RAS 网关感兴趣下一个或多个在以下情况下,：  
  
-   你要为某个组织设计 IT 基础结构或提供相应支持，而该组织正在使用或计划使用 Hyper-V 在虚拟网络中部署虚拟机 (VM)。  
  
-   你要为某个组织设计 IT 基础结构或提供相应支持，而该组织已部署或计划部署云技术。  
  
-   你想要在物理网络与虚拟网络之间提供全面的网络连接。  
  
-   你想要通过 Internet 向你组织的客户提供有权访问其虚拟网络。  
  
-   你想要为对你的组织网络的远程访问你组织的员工提供。  
  
-   你想要通过 Internet 连接在不同物理位置的办公室。  
 
适用于信息技术 (IT) 专业人员，本主题提供有关 RAS 网关，包括 RAS 网关部署模式和功能的概述信息。 
  
本主题包含以下部分。  
  
  
-   [RAS 网关部署模式](#bkmk_modes)  
  
-   [RAS 网关群集实现高可用性](#bkmk_clustering)  
  
-   [RAS 网关功能](#bkmk_features)  
  
-   [RAS 网关部署方案](#bkmk_deploy)  
  
-   [RAS 网关管理工具](#bkmk_manage)  
  


  
## <a name="bkmk_modes"></a>RAS 网关部署模式  
RAS 网关包括以下部署模式。  
  
### <a name="single-tenant-mode"></a>单租户模式下  
对于大多数组织，在单租户模式下使用 RAS 网关是典型的配置。 在单租户模式下，您可以将部署 RAS 网关为边缘 VPN 服务器、 边缘 DirectAccess 服务器，或两者同时。 在此配置中，RAS 网关提供远程员工通过使用 VPN 或 DirectAccess 连接连接到你的网络中。 此外，单租户模式下，可通过 Internet 连接在不同物理位置的办公室。  
  
### <a name="multitenant-mode"></a>多租户模式  
如果你的组织是 CSP 或企业与多个租户，则可以在多租户模式下以网络流量路由到和从虚拟和物理网络部署 RAS 网关。  
  
多租户是云基础结构以支持多个租户的虚拟机工作负荷，尚未将其隔离，而同一基础结构上的所有工作负荷运行的能力。 单个租户的多个工作负载可以互连和接受远程管理，但这些系统不与其他租户的工作负载互连，其他租户也不能远程管理它们。  
  
例如，某家企业使用了许多不同的虚拟子网，每个虚拟子网专门为特定的部门（如研发或会计部门）提供服务。 另举一例，某家 CSP 有许多租户，他们的虚拟子网在同一物理数据中心内处于隔离状态。 在这两种情况下，RAS 网关可以将流量路由到与每个租户同时保持每个租户的设计的隔离。 此功能使支持多租户 RAS 网关。  
  
通过使用 HYPER-V 网络虚拟化，这是一种技术，Windows Server 2012 中引入的和改进 Windows Server 2016 中创建虚拟网络。 RAS 网关集成与 HYPER-V 网络虚拟化，且可以在环境中有效地路由网络流量在有许多不同的客户-隔离的租户虚拟网络在同一数据中心。  
  
HYPER-V 网络虚拟化可部署独立于底层物理网络的虚拟机 (VM) 网络的能力。 VM 网络，组成一个或多个虚拟子网的 IP 子网的确切物理位置分离与虚拟网络拓扑。 因此，可以到云-时保留现有 IP 地址和拓扑在云中轻松移动上的本地子网。 这种保留基础结构的能力使得现有服务能够继续工作，而不考虑子网的物理位置， 也就是说，Hyper-V 网络虚拟化可实现无缝混合云的建立。  
  
> [!NOTE]  
> HYPER-V 网络虚拟化是一种网络叠加技术，使用网络虚拟化基本路由封装 ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00))，它使租户能够享用自己的地址空间并让 Csp 获得可伸缩性更好，比使用 Vlan 进行租户隔离的情况下可行。  
  
在 Windows Server 2016，RAS 网关之间路由网络流量的物理网络和 VM 网络资源，而不管资源位于何处。 RAS 网关可用于在同一物理位置或多个不同的物理位置的物理和虚拟网络之间路由网络流量。  
  
例如，如果在同一物理位置有物理网络和虚拟网络，则可以部署运行 HYPER-V 的使用要充当转发网关和虚拟和物理之间路由流量的 RAS 网关 VM 配置的计算机网络。  
  
在另一个示例中，如果你的虚拟网络存在于云中，CSP 可以部署 RAS 网关，以便可以在 VPN 服务器与 CSP 的 RAS 网关; 之间创建虚拟专用网络 (VPN) 站点到站点连接建立此链接时可以通过 VPN 连接连接到云中的虚拟资源。  
  
有关详细信息，请参阅[RAS 网关高可用性](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_clustering"></a>RAS 网关群集实现高可用性  
在专用计算机正在运行 HYPER-V 且配置有一个 VM 上部署 RAS 网关。 然后，VM 被配置为 RAS 网关。  
  
有关网络资源的高可用性，可以使用故障转移部署 RAS 网关，通过使用两个物理主机服务器运行 HYPER-V 的每个同时运行的虚拟机 (VM) 配置为网关。 然后，可将网关 VM 配置为群集，以便在出现网络中断和硬件故障时提供故障转移保护。  
  
例如，如果你的组织是与私有云部署的企业，可能需要仅有两个 RAS 网关 Vm，其中每个安装在运行 HYPER-V 的不同计算机。 在此方案中，RAS 网关 Vm 添加到群集，以提供高可用性。  
  
在另一个示例中，如果你的组织是通过两百租户置于数据中心，云服务提供商 (CSP) 你可以使用八个 RAS 网关 Vm 群集提供路由服务 50 个租户的 RAS 网关 Vm 的每个对。 在此方案中，两台计算机正在运行的 HYPER-V 每个已配置为 RAS 网关的四个 Vm。 然后，配置四个 RAS 网关 VM 群集，每个群集，其中包含一个虚拟机与运行 HYPER-V 的每台计算机。  
  
当您部署 RAS 网关运行 HYPER-V 和配置为网关必须运行 Windows Server 2012 R2 或 Windows Server 2016 的 Vm 的主机服务器。  
  
## <a name="bkmk_features"></a>RAS 网关功能  
RAS 网关包括以下功能。  
  
-   **站点到站点 VPN**。 此 RAS 网关功能，可通过使用站点到站点 VPN 连接通过 Internet 连接两个网络位于不同物理位置。 如果您具有的总部和多个分支机构，可以部署 RAS 网关，每个位置处的边缘和创建站点到站点连接来提供两个位置之间的网络流量流。 对于托管在自己的数据中心中的多个租户的 Csp，RAS 网关提供多租户网关解决方案，允许租户将访问和管理其资源通过站点到站点 VPN 连接从远程站点，这将允许之间的网络流量在你的数据中心和其物理网络中的虚拟资源。  
  
-   **点到站点 VPN**。 此 RAS 网关功能，组织员工或管理员就可以从远程位置连接到你组织的网络。 对于 RAS 网关的单个租户部署，远程员工可以通过使用 VPN 连接连接到你的组织网络。 此连接允许它们使用的内部网络资源，例如 intranet 网站和文件服务器。 对于多租户部署，租户网络管理员可以使用点到站点 VPN 连接来访问在 CSP 数据中心的虚拟网络资源。  
  
-   **动态路由与边界网关协议 (BGP)**。 BGP 可减少对在路由器上进行手动路由配置的需要，因为它是一种动态路由协议，可自动了解使用站点到站点 VPN 连接进行连接的站点之间的路由。 如果你的组织有多个使用如 RAS 网关启用 BGP 的路由器连接的站点，BGP 允许自动计算并使用彼此发生网络中断或故障时的有效路由的路由器。 有关详细信息，请参阅[RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  
-   **网络地址转换 (NAT)**。 网络地址转换 (NAT)，可与单个公共 IP 地址共享通过单个界面公共 Internet 的连接。 专用网络上的计算机使用专用的不可路由的地址。 NAT 将专用地址映射到公共地址。 此 RAS 网关功能允许具有单个租户部署的组织员工访问从网关后面的 Internet 资源。 适用于 Csp，此功能允许租户 Vm 可以访问 Internet 运行的应用程序。 例如，租户配置为 Web 服务器的 VM 可以与外部财务资源来处理信用卡交易。  

  
## <a name="bkmk_deploy"></a>RAS 网关部署方案  
以下是建议的部署方案的 RAS 网关。  
  
-   **企业边缘的单个租户部署**。 通过单个租户企业部署，你可以连接一个物理到其他多个物理位置在 Internet 上通过站点到站点 VPN 功能-和边界网关协议 (BGP)，可使用动态路由。 您还可以使用点到站点 VPN 连接和 DirectAccess 连接到你的组织网络提供远程员工访问。 （DirectAccess 连接将始终启用，而且还提供的一个优势在于可以轻松地管理连接的计算机使用 DirectAccess，因为只要它们是在和连接到 Internet 的连接。）以便在 Intranet 上的计算机可以轻松地与 Internet 通信，还可以使用 NAT，配置单个租户企业 RAS 网关。  
  
-   **云服务提供商边缘设备的多租户部署**。 RAS 网关适用于 Csp 的多租户部署，可提供给租户的所有功能可用于企业边缘单个租户部署。 在你的数据中心中的租户虚拟网络和租户网络位置跨租户有其云资源的无缝访问所有时间的 Internet 平均值之间的站点到站点 VPN 连接。 点到站点 VPN 访问的租户意味着租户管理员可以始终连接到其数据中心，以管理其资源中的虚拟网络。 BGP 提供动态路由，并使租户连接到他们的资产，即使网络问题发生在 Internet 上或其他位置。 NAT 允许租户 Vm 连接到 Internet，如信用卡处理资源上的资源。  
  
## <a name="bkmk_manage"></a>RAS 网关管理工具  
以下是 RAS 网关的管理工具。  
  
-   在 Windows Server 2016 中，若要部署 RAS 网关路由器，您必须使用 Windows PowerShell 命令。 有关详细信息，请参阅[的远程访问 Cmdlet](https://technet.microsoft.com/library/hh918399.aspx)适用于 Windows Server 2016 和 Windows 10。  
  
-   在 System Center 2012 R2 Virtual Machine Manager (VMM)，RAS 网关名为 Windows Server 网关。 一组有限的边界网关协议 (BGP) 配置选项可在 VMM 软件界面中，包括**BGP 本地 IP 地址**并**自治系统编号 (ASN)**， **BGP 对等 IP 地址的列表**，并**ASN 值**。 但是，你可以使用远程访问 Windows PowerShell BGP 命令来配置 Windows Server 网关的所有其他功能。 有关详细信息，请参阅[Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm)并[的远程访问 Cmdlet](https://technet.microsoft.com/library/hh918399.aspx)适用于 Windows Server 2016 和 Windows 10。  
  
## <a name="related-topics"></a>相关主题
- [RAS 网关高可用性](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Windows Server 中的 GRE 隧道](gre-tunneling-windows-server.md)
- [RAS 网关 GRE 隧道吞吐量和性能](RAS-Gateway-GRE-Perf.md)
