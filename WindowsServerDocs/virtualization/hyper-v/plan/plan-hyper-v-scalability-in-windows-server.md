---
title: Windows Server 2016 和 Windows Server 2019 中的 Hyper-v 可伸缩性规划
description: 列出了可在 Hyper-v 和虚拟机中添加或删除的组件所支持的最大数目，如内存量和虚拟处理器的数量。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 493f7926a6ef686e6d47c1a3120a65ed0799b0db
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2019
ms.locfileid: "71934954"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016-and-windows-server-2019"></a>Windows Server 2016 和 Windows Server 2019 中的 Hyper-v 可伸缩性规划

> 适用于：Windows Server 2016、Windows Server 2019
  
本文详细介绍了可在 Hyper-v 主机或其虚拟机（例如虚拟处理器或检查点）上添加和删除的组件的最大配置。 规划部署时，请考虑适用于每个虚拟机的最大配置，以及应用于 Hyper-v 主机的最大配置。 

内存和逻辑处理器的最大数量是 Windows Server 2012 的最大增加，以响应支持计算机学习和数据分析等更新方案的请求。 Windows Server 博客最近发布了一个虚拟机的性能结果，其中包含 5.5 tb 的内存和128个运行 4 TB 内存中数据库的虚拟处理器。 性能大于物理服务器的 95%。 有关详细信息，请参阅[Windows Server 2016 hyper-v 大规模 VM 性能，用于内存中事务处理](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 其他数字类似于适用于 Windows Server 2012 的数字。 0Maximums for Windows Server 2012 R2 与 Windows Server 2012 相同。 \) @no__t 
  
> [!NOTE]  
> 有关 System Center Virtual Machine Manager (VMM) 的信息，请参阅 [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm)。 VMM 是单独出售的用于管理虚拟化数据中心的 Microsoft 产品。  
  
## <a name="maximums-for-virtual-machines"></a>最大虚拟机  
这些最大的应用于每个虚拟机。 并非所有组件都可用于两代虚拟机。 有关生成的比较，请参阅是否[应在 hyper-v 中创建第1代或第2代虚拟机？](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|组件|最多|说明|  
|-------------|-----------|---------|  
|检查点|50|实际数量可能较少，这取决于可用的存储。 每个检查点都存储为使用物理存储的 .avhd 文件。|  
|内存|对于第2代为 12 TB; <br>1 TB，适用于第1代|复查特定操作系统的要求，以确定最小数量和推荐的数量。|  
|串行 (COM) 端口|2|无。|  
|直接连接到虚拟机的物理磁盘的大小|变化不定|最大大小由来宾操作系统决定。|  
|虚拟光纤通道适配器|4|作为最佳实践，我们建议你将每个虚拟光纤通道适配器连接到不同的虚拟 SAN。|  
|虚拟软盘设备|1 个虚拟软盘驱动器|无。|
|虚拟硬盘容量|64 TB，适用于 VHDX 格式;<br>2040 GB 用于 VHD 格式|每个虚拟硬盘都将作为 .vhdx 或 .vhd 文件存储在物理媒体上，具体取决于虚拟硬盘所使用的格式。|  
|虚拟 IDE 磁盘|4|启动磁盘（有时称为启动磁盘）必须连接到某个 IDE 设备。 启动盘可以是虚拟硬盘，也可以是与虚拟机直接相连的物理磁盘。|  
|虚拟处理器|第2代为 240;<br>第1代为 64;<br>320可用于主机操作系统（根分区）|来宾操作系统支持的虚拟处理器数量可能会减少。 有关详细信息，请参阅针对特定操作系统发布的信息。|
|虚拟 SCSI 控制器|4|使用虚拟 SCSI 设备需要集成服务，这些服务可用于支持的来宾操作系统。 有关支持的操作系统的详细信息，请参阅[支持的 Linux 和 FreeBSD 虚拟机](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)和[受支持的 Windows 来宾操作系统](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)。|  
|虚拟 SCSI 磁盘|256|各 SCSI 控制器最多可支持 64 个磁盘，这意味着各虚拟机可具有多达 256 个虚拟 SCSI 磁盘的配置。 （4 个控制器 x 64 个磁盘/控制器）|  
|虚拟网络适配器|Windows Server 2016 支持总共12个：<br> -8 个特定于 hyper-v 的网络适配器<br>-4 旧版网络适配器 <br> Windows Server 2019 支持68总计： <br> -64 hyper-v 特定网络适配器<br>-4 旧版网络适配器  |Hyper-v 特定网络适配器提供更好的性能，并需要包含在 integration services 中的驱动程序。 有关详细信息，请参阅[规划 Windows Server 中的 hyper-v 网络](plan-hyper-v-networking-in-windows-server.md)。|  
  
## <a name="maximums-for-hyper-v-hosts"></a>最大为 Hyper-v 主机  
这些最大的适用于每个 Hyper-v 主机。  
  
|组件|最多|说明|  
|-------------|-----------|---------|  
|逻辑处理器|512|这两个必须在固件中启用：<br /><br />-硬件辅助虚拟化<br />-硬件强制实施的数据执行保护（DEP）<br /><br />主机操作系统（根分区）将只看到最多320逻辑处理器|  
|内存|24 TB|无。|  
|网络适配器组（NIC 组合）|Hyper-V 无限制。|有关详细信息，请参阅[NIC 组合](../../../networking/technologies/nic-teaming/NIC-Teaming.md)。|  
|物理网络适配器|Hyper-V 无限制。|无。|  
|每台服务器的正在运行的虚拟机|1024|无。|  
|存储|受主机操作系统支持的限制。 Hyper-V 无限制。|**注意：** 使用 SMB 3.0 时，Microsoft 支持网络连接存储（NAS）。 不支持基于 NFS 的存储。|
|每服务器的虚拟网络开关端口|视情况而定；Hyper-V 无任何限制。|实际限制取决于可用的计算资源。|  
|每个逻辑处理器的虚拟处理器数|Hyper-V 无比率。|无。|  
|每台服务器的虚拟处理器|2048|无。|  
|虚拟存储区域网络 (SAN)|Hyper-V 无限制。|无。|  
|虚拟交换机|视情况而定；Hyper-V 无任何限制。|实际限制取决于可用的计算资源。|  
 
## <a name="failover-clusters-and-hyper-v"></a>故障转移群集和 Hyper-V  
此表列出了使用 Hyper-v 和故障转移群集时应用的最大的列。 执行容量规划非常重要，目的是确保有足够的硬件资源来运行群集环境中的所有虚拟机。  

若要了解故障转移群集的更新（包括虚拟机的新功能），请参阅[Windows Server 2016 中故障转移群集的新增](../../../failover-clustering/whats-new-in-failover-clustering.md)功能。

|组件|最多|说明|  
|-------------|-----------|---------|  
|每群集的节点|64|请考虑为故障转移及为维护任务（如应用更新）保留的节点数量。 我们建议你计划使用足够的资源为故障转移保留 1 个节点，即在另一个节点故障转移到该节点之前，该节点一直保持空闲。 （这有时被称为被动节点。）如果你希望保留更多节点，则可以增加此数量。 没有建议的保留节点与活动节点的比率或乘数;唯一的要求是，群集中的节点总数不能超过最大值64。|  
|每个群集中和每个节点上运行的虚拟机数|每个群集 8,000 个|多个因素可能会影响可在一个节点上同时运行的实际虚拟机数，例如：<br />-每个虚拟机正在使用的物理内存量。<br />-网络和存储带宽。<br />-磁盘主轴的数量，影响磁盘 i/o 性能。|  
  

