---
title: Windows Server 中的 HYPER-V 网络规划
description: 介绍什么所需的基本网络的 HYPER-V 中，并提供说明链接
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 83c014b917566a78796d061dd88962966ec0c9ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829868"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Windows Server 中的 HYPER-V 网络规划

>适用于：Microsoft HYPER-V Server 2016，Windows Server 2016 中，Microsoft HYPER-V Server 2019，Windows Server 2019
  
中的 HYPER-V 网络的一个基本了解可帮助你规划网络以便将虚拟机。 使用实时迁移时，并与其他服务器功能和角色使用的 HYPER-V 时，本文还介绍了一些网络注意事项。  
  
## <a name="hyper-v-networking-basics"></a>HYPER-V 网络基本原理  
在 HYPER-V 中的基本网络是相当简单。 它使用两个部分的虚拟交换机和虚拟网络适配器。 你将需要至少各项之一来建立虚拟机的网络。 虚拟交换机连接到任何基于以太网的网络。 虚拟网络适配器连接到虚拟交换机，这样就可以使用网络为虚拟机上的端口。  
  
建立基本的网络连接的最简单方法是在安装 HYPER-V 时创建的虚拟交换机。 然后，当你创建虚拟机，可以将其连接到交换机。 自动连接到交换机将虚拟网络适配器添加到虚拟机。 有关说明，请参阅[创建的虚拟交换机的 HYPER-V 虚拟机。](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。  
  
若要处理不同类型的网络，可以添加虚拟交换机和虚拟网络适配器。 所有交换机都均属于的 HYPER-V 主机，但每个虚拟网络适配器属于只有一台虚拟机。  
  
虚拟交换机是基于软件的第 2 层以太网网络交换机。 它提供用于监视、 控制，并将流量，以及安全性和诊断的内置功能。  可以通过安装插件，也称为添加到内置功能的一套*扩展*。 这些是可从独立软件供应商。 有关交换机和扩展插件的详细信息，请参阅[HYPER-V 虚拟交换机](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
### <a name="switch-and-network-adapter-choices"></a>开关和网络适配器选择  
HYPER-V 提供了三种类型的虚拟交换机和两种类型的虚拟网络适配器。 你将选择每个时创建它，你需要哪的一个。 可以使用 Hyper-v 管理器或 Windows PowerShell 的 HYPER-V 模块来创建和管理虚拟交换机和虚拟网络适配器。 一些高级的网络功能，如扩展的端口访问控制列表 (Acl)，只能管理使用 HYPER-V 模块中的 cmdlet。  
  
在创建后，可以对虚拟交换机或虚拟网络适配器进行一些更改。 例如，可以将现有的交换机更改为不同类型，但执行该操作会影响连接到该交换机的所有虚拟机的网络功能。  因此，你可能不会执行此操作，除非你出现了错误或需要进行某些测试。 作为另一个示例中，您可以连接到不同的交换机，如果你想要连接到不同的网络可能会执行的虚拟网络适配器。 但是，您不能从一种类型到另一个更改虚拟网络适配器。 而不是更改的类型，将添加另一个虚拟网络适配器，并选择适当的类型。  
  
虚拟交换机类型包括：  
  
-   **外部虚拟交换机**-连接到有线的物理网络通过绑定到物理网络适配器。  
  
-   **内部虚拟交换机**-连接到的网络，可以仅由运行具有的虚拟交换机在主机和主机和虚拟机之间的虚拟机。  
  
-   **专用虚拟交换机**-连接到虚拟交换机，但不提供主机和虚拟机之间的网络在主机上运行的虚拟机可以仅使用一个网络。  
  
虚拟网络适配器类型为：  
  
-   **HYPER-V 特定网络适配器**-适用于第 1 代和第 2 代虚拟机。 它专为 HYPER-V，并且需要 HYPER-V 集成服务中包含的驱动程序。 此类型的速度更快的网络适配器和是推荐的选择，除非你需要启动到网络或正在运行不受支持的来宾操作系统。 仅适用于受支持的来宾操作系统提供所需的驱动程序。 请注意，在 Hyper-v 管理器和网络 cmdlet，此类型只是称为网络适配器。  
  
-   **旧版网络适配器**-仅在第 1 代虚拟机中可用。 模拟 Intel 21140 基于 PCI 快速以太网适配器，并可以用于启动到网络因此可以从 Windows 部署服务之类的服务来安装操作系统。  
  
## <a name="hyper-v-networking-and-related-technologies"></a>HYPER-V 网络连接和相关的技术  
最新的 Windows Server 版本引入了改进，使您可以配置适用于 HYPER-V 的网络的更多选项。 例如，Windows Server 2012 引入了支持的聚合网络。 这样就可以将通过一个外部虚拟交换机的网络流量路由。 Windows Server 2016 生成对此网络适配器绑定到 HYPER-V 虚拟交换机上，从而远程直接内存访问 (RDMA)。 使用或无需切换嵌入组合 (SET)，可以使用此配置。 有关详细信息，请参阅[远程直接内存访问&#40;RDMA&#41;和交换机嵌入式组合&#40;设置&#41;](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
某些功能依赖于特定网络配置或在某些配置更好地完成。 规划或更新网络基础结构时，请考虑这些。  
  
**故障转移群集**-它是在虚拟交换机上隔离群集流量和使用的 HYPER-V 服务质量 (QoS) 是最佳做法。 有关详细信息，请参阅[HYPER-V 群集的网络建议](https://technet.microsoft.com/library/dn550728.aspx)  
  
**实时迁移**-性能选项用于减少网络和 CPU 使用率以及完成实时迁移所需的时间。 有关说明，请参阅[设置为没有故障转移群集的实时迁移的主机](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)。  
  
**存储空间直通**-此功能依赖于 SMB3.0 网络协议和 RDMA。 有关详细信息，请参阅[存储空间直通在 Windows Server 2016 中](../../../storage/storage-spaces/storage-spaces-direct-overview.md)。