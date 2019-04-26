---
title: 规划 Windows Server 2016 中的 HYPER-V 可伸缩性
description: 支持的组件可以添加或删除从 HYPER-V 和虚拟机数的最大的列表，如内存量和虚拟处理器数目。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 03a464269c8aea29c226dee776f0dfacfe48743a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850258"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>规划 Windows Server 2016 中的 HYPER-V 可伸缩性

> 适用于：Windows Server 2016
  
本文提供有关可以添加和删除的 HYPER-V 主机或其虚拟机，例如虚拟处理器或检查点上的组件的最大配置的详细信息。 规划你的部署，请考虑将应用于每个虚拟机，以及那些适用于 HYPER-V 主机的最大配置。 

有关内存和逻辑处理器的最大值是从 Windows Server 2012 的最大增加到请求，以支持较新方案，例如机器学习和数据分析的响应中。 Windows Server 博客最近发布的虚拟机的 5.5 万亿字节的内存和 128 个虚拟处理器运行 4 TB 内存中数据库的性能结果。 性能   95%的物理服务器的性能。 有关详细信息，请参阅[内存中事务处理的 Windows Server 2016 HYPER-V 大规模 VM 性能](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 其他数字是类似于适用于 Windows Server 2012。 \(适用于 Windows Server 2012 R2 的最大值是为 Windows Server 2012 相同的。\) 
  
> [!NOTE]  
> 有关 System Center Virtual Machine Manager (VMM) 的信息，请参阅 [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm)。 VMM 是单独出售的用于管理虚拟化数据中心的 Microsoft 产品。  
  
## <a name="maximums-for-virtual-machines"></a>为虚拟机的最大值  
这些最大值适用于每个虚拟机。 并非所有组件都均位于这两个代虚拟机。 代的比较，请参阅[应在 HYPER-V 中创建第 1 或 2 代虚拟机？](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|组件|最多|说明|  
|-------------|-----------|---------|  
|检查点|50|实际数量可能较少，这取决于可用的存储。 每个检查点存储为.avhd 文件使用的物理存储。|  
|内存|有关第 2 代; 12 TB <br>1 TB 的第 1 代|复查特定操作系统的要求，以确定最小数量和推荐的数量。|  
|串行 (COM) 端口|2|无。|  
|直接连接到虚拟机的物理磁盘的大小|变化不定|最大大小由来宾操作系统决定。|  
|虚拟光纤通道适配器|4|作为最佳实践，我们建议你将每个虚拟光纤通道适配器连接到不同的虚拟 SAN。|  
|虚拟软盘设备|1 个虚拟软盘驱动器|无。|
|虚拟硬盘容量|64 TB 的 VHDX 格式;<br>VHD 格式的的 2040 GB|每个虚拟硬盘都将作为 .vhdx 或 .vhd 文件存储在物理媒体上，具体取决于虚拟硬盘所使用的格式。|  
|虚拟 IDE 磁盘|4|启动磁盘 （有时称为启动磁盘） 必须附加到 IDE 设备之一。 启动盘可以是虚拟硬盘，也可以是与虚拟机直接相连的物理磁盘。|  
|虚拟处理器|240，第 2 代;<br>对于生成 1; 为 64<br>320 供主机操作系统 （根分区）|来宾操作系统支持的虚拟处理器数量可能会减少。 有关详细信息，请参阅针对特定操作系统发布的信息。|
|虚拟 SCSI 控制器|4|使用虚拟 SCSI 设备都需要集成服务，可用于受支持的来宾操作系统。 支持操作系统的详细信息，请参阅[支持的 Linux 和 FreeBSD 虚拟机](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)并[支持 Windows 来宾操作系统](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)。|  
|虚拟 SCSI 磁盘|256|各 SCSI 控制器最多可支持 64 个磁盘，这意味着各虚拟机可具有多达 256 个虚拟 SCSI 磁盘的配置。 （4 个控制器 x 64 个磁盘/控制器）|  
|虚拟网络适配器|总共 12 个：<br> -8 的 HYPER-V 特定网络适配器<br>-4 个旧版网络适配器|HYPER-V 特定网络适配器提供了更好的性能，需要包含 integration services 中的驱动程序。 有关详细信息，请参阅[规划 Windows Server 中的 HYPER-V 网络](plan-hyper-v-networking-in-windows-server.md)。|  
  
## <a name="maximums-for-hyper-v-hosts"></a>对于 HYPER-V 主机的最大值  
这些最大值将应用到每个 HYPER-V 主机。  
  
|组件|最多|说明|  
|-------------|-----------|---------|  
|逻辑处理器|512|必须在固件中启用这两种：<br /><br />-硬件辅助虚拟化<br />-硬件强制的数据执行保护 (DEP)<br /><br />主机操作系统 （根分区） 将只看到最大 320 个逻辑处理器|  
|内存|24 TB|无。|  
|网络适配器组（NIC 组合）|Hyper-V 无限制。|有关详细信息，请参阅[NIC 组合](../../../networking/technologies/nic-teaming/NIC-Teaming.md)。|  
|物理网络适配器|Hyper-V 无限制。|无。|  
|每台服务器的正在运行的虚拟机|1024|无。|  
|存储|受限制的主机操作系统支持的功能。 Hyper-V 无限制。|**注意：** 使用 SMB 3.0 时，Microsoft 支持网络连接存储 (NAS)。 不支持基于 NFS 的存储。|
|每服务器的虚拟网络开关端口|视情况而定；Hyper-V 无任何限制。|实际限制取决于可用的计算资源。|  
|每个逻辑处理器的虚拟处理器数|Hyper-V 无比率。|无。|  
|每台服务器的虚拟处理器|2048|无。|  
|虚拟存储区域网络 (SAN)|Hyper-V 无限制。|无。|  
|虚拟交换机|视情况而定；Hyper-V 无任何限制。|实际限制取决于可用的计算资源。|  
 
## <a name="failover-clusters-and-hyper-v"></a>故障转移群集和 Hyper-V  
此表列出了使用 HYPER-V 和故障转移群集时，适用的最大配置。 请务必执行容量规划以确保将硬件资源不足，无法在群集环境中运行的所有虚拟机。  

若要了解有关更新故障转移群集，包括新功能对于虚拟机，请参阅[What's New in Windows Server 2016 中故障转移群集](../../../failover-clustering/whats-new-in-failover-clustering.md)。

|组件|最多|说明|  
|-------------|-----------|---------|  
|每群集的节点|64|请考虑为故障转移及为维护任务（如应用更新）保留的节点数量。 我们建议你计划使用足够的资源为故障转移保留 1 个节点，即在另一个节点故障转移到该节点之前，该节点一直保持空闲。 （这有时被称为被动节点。）如果你希望保留更多节点，则可以增加此数量。 没有推荐的比率或倍数保留节点到活动节点;唯一要求是在群集中的节点总数不能超过 64 个最大值。|  
|每个群集中和每个节点上运行的虚拟机数|每个群集 8,000 个|以下几个因素会影响虚拟机，你可以运行在同一时间在一个节点上，如实数：<br />-正在使用的每个虚拟机的物理内存量。<br />-网络和存储带宽。<br />-磁盘主轴数量，这会影响磁盘 I/O 性能。|  
  

