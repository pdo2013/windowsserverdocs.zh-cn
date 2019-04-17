---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: "Windows Server 中存储方面的新增功能"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: kumudd
ms.date: 09/15/2016
ms.openlocfilehash: 9aab6246f7ddc86629834bf20a7d21cc4ce2ec8f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-storage-in-windows-server-2016"></a>Windows Server 2016 中存储方面的新增功能

>适用于：WindowsServer 2016

本主题介绍 Windows 10 2016 中存储方面的新功能和更改的功能。

## <a name="s2d"></a>Storage Spaces Direct  
存储空间直通允许通过使用具有本地存储的服务器构建高可用性和可缩放存储。 该功能简化了软件定义的存储系统的部署和管理并且允许使用 SATA SSD 和 NVMe 磁盘设备等新型磁盘设备，而之前群集存储空间无法使用共享磁盘。  

**这一更改增添了什么价值？**  
空间存储直通使服务提供商和企业可使用带本地存储的行业标准服务器来构建高可用性和高扩展性的软件定义的存储。 使用带本地存储的服务器可降低复杂性、增强可伸缩性，并允许使用之前不可能使用的存储设备，如使用 SATA 固态磁盘降低闪存存储成本或使用 NVMe 固态磁盘实现更加性能。  

空间存储直通不再需要共享 SAS 结构，从而简化了部署和配置。 它改为使用网络作为存储结构，利用 SMB3 和 SMB 直通 (RDMA) 实现高速、低延迟的 CPU 高效存储。 若要横向扩展，只需添加更多服务器以增加存储容量和 I/O 性能  
有关详细信息，请参阅 [Windows Server 2016 中的存储空间直通](storage-spaces/storage-spaces-direct-overview.md).  

**工作原理的不同之处是什么？**  
此功能是 Windows Server 2016 的新增功能。  

## <a name="storage-replica"></a>存储副本  
存储副本可在各个服务器或群集之间实现存储不可知的块级同步复制，以便在站点间进行灾难恢复及故障转移群集扩展。 同步复制支持物理站点中的镜像数据和在崩溃时保持一致的卷，以确保文件系统级别的数据损失为零。 异步复制允许超出都市范围、可能存在数据损失的站点扩展。  

**这一更改增添了什么价值？**  
使用存储复制可执行下列操作：  

* 为关键任务工作负荷的计划内和计划外中断提供单一供应商灾难恢复解决方案。
* 使用具有广为赞誉的可靠性、可伸缩性和高性能的 SMB3 传输。
* 将 Windows 故障转移群集扩展到都市距离。
* 将端到端的 Microsoft 软件用于存储和群集（例如 Hyper-V、存储复本、存储空间、群集、向外扩展文件服务器、SMB3、重复数据删除和 ReFS/NTFS）。
* 可帮助降低成本和复杂性，如下所示： 
    * 与硬件无关，对特定存储配置（例如 DAS 或 SAN）没有要求。
    * 允许使用商品存储和网络技术。
    * 通过故障转移群集管理器轻松对单独的节点和群集进行图形管理的功能。
    * 包括通过 Windows PowerShell 的全面、大型的脚本选项。 
* 有助于减少停机时间，提高可靠性和 Windows 内部的工作效率。  
* 提供支持能力、性能度量标准和诊断功能。  

有关详细信息，请参阅 [Windows Server 2016 中的存储副本](storage-replica/storage-replica-overview.md)。  

**工作原理的不同之处是什么？**  
此功能是 Windows Server 2016 的新增功能。  

## <a name="storage-qos"></a>存储服务质量  
现在可以使用存储服务质量 (QoS) 来集中监控端到端存储性能，并使用 Windows Server 2016 中的 Hyper-V 和 CSV 群集创建策略。  

**这一更改增添了什么价值？**  
你现在可以在 CSV 群集上创建存储 QoS 策略，并将它们分配给 Hyper-V 虚拟机上的一个或多个虚拟磁盘。 存储性能将随工作负载和存储负载波动自动重新调整以符合策略。  

* 每个策略可以指定保留（最小）和/或限制（最大），以应用于数据流集合，例如虚拟硬盘、单个虚拟机或虚拟机组、服务或租户的集合。  
* 使用 Windows PowerShell 或 WMI 可以执行以下任务：  
    * 在 CSV 群集上创建策略。
    * 枚举 CSV 群集上的可用策略。
    * 将策略分配给 Hyper-V 虚拟机的虚拟硬盘。 
    * 监控每个流的性能和策略中的状态。  
* 如果多个虚拟硬盘共享同一策略，性能将进行公平分配，在策略的最小和最大设置内满足需求。 因此，策略可用于管理一个虚拟硬盘、一个虚拟机、构成服务的多个虚拟机或租户拥有的所有虚拟机。  

**工作原理的不同之处是什么？**  
此功能是 Windows Server 2016 的新增功能。 管理最小预留，通过单个命令监视跨群集的所有虚拟磁盘流，在早期版本的 Windows Server 中无法实现基于策略的集中式管理。  

有关详细信息，请参阅[存储服务质量](storage-qos/storage-qos-overview.md)

## <a name="dedup"></a>重复数据删除  
| 功能 | 新功能或更新功能 | 说明 |
|---------------|----------------|-------------|
| [支持大型卷](data-deduplication/whats-new.md#large-volume-support) | 已更新 | 在 Windows Server 2016 之前，必须专门调整卷的大小实现预期改动，大小超过 10 TB 的卷不适合进行重复数据删除。 在 Windows Server 2016 中，重复数据删除支持**高达 64 TB** 的卷大小。 |
| [支持大型文件](data-deduplication/whats-new.md#large-file-support) | 已更新 | 在 Windows Server 2016 之前，大小接近 1 TB 的文件不适合进行重复数据删除。 在 Windows Server 2016 中，完全支持**高达 1 TB** 的文件。 |
| [支持 Nano Server](data-deduplication/whats-new.md#nano-server-support) | “新建” | 重复数据删除在 Windows Server 2016 的新 Nano Server 部署选项中可用且完全受支持。 |
| [简化的备份支持](data-deduplication/whats-new.md#simple-backup-support) | “新建” | 在 Windows Server 2012 R2 中，通过一系列手动配置步骤支持虚拟化备份应用程序，如 Microsoft 的 [Data Protection Manager](https://technet.microsoft.com/en-us/library/hh758173.aspx) 在 Windows Server 2016 中，已针对虚拟化备份应用程序的重复数据删除的无缝部署添加了新的默认使用类型“备份”。 |
| [支持群集操作系统滚动升级](data-deduplication/whats-new.md#cluster-upgrade-support) | “新建” | 重复数据删除完全支持 Windows Server 2016 的新功能[群集操作系统滚动升级](..//failover-clustering/cluster-operating-system-rolling-upgrade.md)。 |

## <a name="smb-hardening-improvements"></a>SMB 针对 SYSVOL 和 NETLOGON 连接的强化改进  
在到默认的 Active Directory 域服务 SYSVOL 和 NETLOGON 的 Windows 10 和 Windows Server 2016 客户端连接中，域控制器上的共享现在要求 SMB 签名和相互身份验证（例如 Kerberos)。   

**这一更改增添了什么价值？**  
此更改降低了中间人攻击的可能性。   

**作用有何不同？**  
如果 SMB 签名和相互身份验证都不可用，Windows 10 或 Windows Server 2016 计算机不会处理基于域的组策略和脚本。  

> [!NOTE]  
> 这些设置的注册表值默认情况下并不出现，但在被组策略或其他注册表值替代前，强化规则仍然适用。  

有关这些安全改进（也称为 UNC 强化）的详细信息，请参阅 Microsoft 知识库文章 [3000483](http://support.microsoft.com/kb/3000483) 和 [MS15-011 & MS15-014：强化组策略](http://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy)。  

## <a name="work-folders"></a>工作文件夹
改进了工作文件夹服务器正在运行 Windows Server 2016 并且工作文件夹客户端是 Windows 10 时的更改通知。

**这一更改增添了什么价值？**<br>
对于 Windows Server 2012 R2，当文件更改同步到工作文件夹服务器上时，不向客户端通知这一更改并等待 10 分钟获取更新。  在使用 Windows Sever 2016 时，工作文件夹服务器会立即通知 Windows 10 客户端并立即同步文件更改。

**工作原理的不同之处是什么？**<br>
此功能是 Windows Server 2016 的新增功能。 这要求 Windows Server 2016 工作文件夹服务器和客户端必须是 Windows 10。

如果你使用的是较旧客户端或工作文件夹服务器为 Windows Server 2012 R2，则客户端将继续每 10 分钟轮询一次更改。

## <a name="refs"></a>ReFS 
下一次 ReFS 迭代提供对具有各种工作负荷的大规模存储部署的支持，为你的数据提供可靠性、复原能力和可扩展性。     

**这一更改增添了什么价值？**<br>
ReFS 做了以下改进：

* ReFS 实现了新存储层功能，帮助提供更快的性能和更大的存储容量。 此新功能将启用以下内容：
    * 在同一虚拟磁盘上使用多个复原类型（例如，使用性能层中的镜像和容量层中的奇偶校验）。
    * 提高了对偏离工作集的响应能力。 
    * SMR（叠瓦式磁记录）媒体支持。 
* 块克隆的引入大大提高了 VM 操作（例如，.vhdx 检查点合并操作）的性能。
* 新的 ReFS 扫描工具支持泄露存储的恢复，并可帮助回收数据避免严重损坏。 

**工作原理的不同之处是什么？**<br>
这些功能是 Windows Server 2016 中的新增功能。 

## <a name="see-also"></a>另请参阅  
* [Windows Server 2016 中的新增功能](../get-started/what-s-new-in-windows-server-2016.md)  
