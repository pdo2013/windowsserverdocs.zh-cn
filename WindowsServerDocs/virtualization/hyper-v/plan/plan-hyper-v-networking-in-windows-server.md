---
title: 规划 Windows Server 中的 Hyper-v 网络
description: 介绍 Hyper-v 中的基本网络所需的内容，并提供指向说明的链接
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f09bc82d0dd47d3393dd05dcf03913db11e4c335
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392503"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>规划 Windows Server 中的 Hyper-v 网络

>适用于：Microsoft Hyper-V Server 2016、Windows Server 2016、Microsoft Hyper-V Server 2019、Windows Server 2019
  
有关 Hyper-v 中的网络的基本理解有助于规划虚拟机的网络。 本文还介绍了使用实时迁移和将 Hyper-v 与其他服务器功能和角色配合使用时的一些网络注意事项。  
  
## <a name="hyper-v-networking-basics"></a>Hyper-v 网络基础知识  
Hyper-v 中的基本网络非常简单。 它使用两个部分：虚拟交换机和虚拟网络适配器。 你至少需要其中一项才能为虚拟机建立网络连接。 虚拟交换机连接到任何基于以太网的网络。 虚拟网络适配器连接到虚拟交换机上的端口，这使得虚拟机可以使用网络。  
  
建立基本网络的最简单方法是在安装 Hyper-v 时创建虚拟交换机。 然后，当你创建虚拟机时，你可以将其连接到交换机。 连接到交换机会自动将虚拟网络适配器添加到虚拟机中。 有关说明，请参阅为[hyper-v 虚拟机创建虚拟交换机](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。  
  
若要处理不同类型的网络，可以添加虚拟交换机和虚拟网络适配器。 所有交换机都属于 Hyper-v 主机，但是每个虚拟网络适配器仅属于一个虚拟机。  
  
虚拟交换机是基于软件的第2层以太网网络交换机。 它提供用于监视、控制和分割流量的内置功能，以及安全和诊断。  您可以通过安装插件（也称为*扩展*）来向内置功能集添加。 它们可从独立软件供应商处获取。 有关交换机和扩展的详细信息，请参阅[Hyper-v 虚拟交换机](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
### <a name="switch-and-network-adapter-choices"></a>交换机和网络适配器选择  
Hyper-v 提供了三种类型的虚拟交换机和两种类型的虚拟网络适配器。 你将在创建每个所需的时选择一个。 你可以使用 Hyper-v 管理器或 Windows PowerShell 的 Hyper-v 模块来创建和管理虚拟交换机和虚拟网络适配器。 某些高级网络功能，例如扩展端口访问控制列表（Acl），只能通过使用 Hyper-v 模块中的 cmdlet 进行管理。  
  
创建虚拟交换机或虚拟网络适配器后，可以对其进行一些更改。 例如，可以将现有交换机更改为不同的类型，但这样做会影响连接到该交换机的所有虚拟机的网络功能。  因此，您可能不会这样做，除非您犯了错误或需要测试某些内容。 作为另一个示例，你可以将虚拟网络适配器连接到不同的交换机，如果你想要连接到其他网络，则可能会这样做。 但不能将虚拟网络适配器从一种类型更改为另一种类型。 添加另一个虚拟网络适配器，并选择适当的类型，而不是更改类型。  
  
虚拟交换机类型为：  
  
-   **外部虚拟交换机**-通过绑定到物理网络适配器连接到有线物理网络。  
  
-   **内部虚拟交换机**-连接到只能由在具有虚拟交换机的主机上运行的虚拟机以及主机和虚拟机之间使用的网络。  
  
-   **专用虚拟交换机**-连接到只能由在具有虚拟交换机的主机上运行的虚拟机使用的网络，但不提供主机和虚拟机之间的网络连接。  
  
虚拟网络适配器类型为：  
  
-   **Hyper-v 特定网络适配器**-适用于第1代和第2代虚拟机。 它专门设计用于 Hyper-v，并需要包含在 Hyper-v integration services 中的驱动程序。 此类网络适配器的速度更快，但建议选择此选项，除非你需要启动到网络或运行不受支持的来宾操作系统。 仅为支持的来宾操作系统提供所需的驱动程序。 请注意，在 Hyper-v 管理器和网络 cmdlet 中，此类型仅称为网络适配器。  
  
-   **旧版网络适配器**-仅在第1代虚拟机中可用。 模拟基于 Intel 21140 的 PCI 快速以太网适配器，可用于启动到网络，以便可以从 Windows 部署服务的服务安装操作系统。  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Hyper-v 网络和相关技术  
最近的 Windows Server 版本引入了更多用于为 Hyper-v 配置网络的选项。 例如，Windows Server 2012 引入了对聚合网络的支持。 这使你可以通过一个外部虚拟交换机路由网络流量。 Windows Server 2016 通过允许绑定到 Hyper-v 虚拟交换机的网络适配器上的远程直接内存访问（RDMA）来构建。 可以将此配置用于或不使用交换机嵌入组合（SET）。 有关详细信息，请参阅[远程直接&#40;内存&#41;访问 RDMA 和交换机&#40;嵌入式&#41;组合集](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
某些功能依赖于特定的网络配置，或在某些配置下性能更佳。 在规划或更新网络基础结构时，请考虑以下事项。  
  
**故障转移群集**-在虚拟交换机上隔离群集流量并使用 Hyper-v 服务质量（QoS）的最佳做法。 有关详细信息，请参阅[Hyper-v 群集的网络建议](https://technet.microsoft.com/library/dn550728.aspx)  
  
**实时迁移**-使用性能选项来减少网络和 CPU 使用率，以及完成实时迁移所花费的时间。 有关说明，请参阅在[没有故障转移群集的情况下，设置用于实时迁移的主机](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)。  
  
**存储空间直通**-此功能依赖于 smb 3.0 网络协议和 RDMA。 有关详细信息，请参阅[Windows Server 2016 中的存储空间直通](../../../storage/storage-spaces/storage-spaces-direct-overview.md)。