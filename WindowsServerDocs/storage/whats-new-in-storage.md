---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Windows Server 中存储方面的新增功能
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 05/29/2019
ms.openlocfilehash: f72156b050aa943cfafaf1fa2539911d6d1e089e
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501483"
---
# <a name="whats-new-in-storage-in-windows-server"></a>什么是 Windows Server 中存储中的新增功能

>适用于：Windows Server 2019，Windows Server 2016 中，Windows Server （半年频道）

本主题介绍了在 Windows Server 2019，Windows Server 2016 中的存储中的新功能和更改功能和 Windows Server 半年频道发布。

## <a name="whats-new-in-storage-in-windows-server-version-1903"></a>什么是 Windows Server，版本 1903年中存储中的新增功能

此版本的 Windows Server 添加了以下更改和技术。

### <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>本地帐户、 群集和 Linux 服务器，现在将迁移存储迁移服务

存储迁移服务，可以更轻松地迁移到较新版本的 Windows Server 的服务器。 它提供了一个图形工具，列出服务器上的数据的清单，然后将数据和配置传输至较新的服务器，同时应用程序或用户无需更改任何内容。

当使用此版本的 Windows Server 协调迁移，我们添加了以下功能：

- 将本地用户和组迁移到新服务器
- 从故障转移群集迁移存储
- 使用 Samba 的 Linux 服务器从迁移存储
- 更轻松地使用 Azure 文件同步到 Azure 中同步已迁移的共享
- 将迁移到 Azure 等的新网络

有关存储迁移服务的详细信息，请参阅[存储迁移服务概述](storage-migration-service/overview.md)。

### <a name="system-insights-disk-anomaly-detection"></a>系统 Insights 磁盘异常情况检测

[系统 Insights](../manage/system-insights/overview.md)是预测分析功能，本地 Windows Server 系统数据进行分析，并提供深入的服务器的运行状况。 它还提供了许多内置功能，但我们已添加可以安装其他功能通过 Windows Admin Center，以从磁盘异常情况检测。

磁盘异常情况检测是当磁盘的运行情况符合突出显示的新功能*以不同方式*比平常。 虽然不同并不一定是您的系统上进行问题排查时可以提供帮助是坏事，查看这些异常的情形。

此功能也是可用于运行 Windows Server 2019 的服务器。

### <a name="windows-admin-center-enhancements"></a>Windows Admin Center 增强功能

Windows Admin Center 的新版本，是添加到 Windows Server 的新功能。 有关最新功能的信息，请参见[Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1809"></a>什么是 Windows Server 2019 和 Windows Server 版本 1809年中使用存储中的新增功能

此版本的 Windows Server 添加了以下更改和技术。

### <a name="manage-storage-with-windows-admin-center"></a>使用 Windows Admin Center 管理存储

[Windows Admin Center](../manage/windows-admin-center/overview.md)是用于管理服务器、 群集、 存储空间直通和 Windows 10 Pc 中使用超聚合基础结构的新本地部署的、 基于浏览器的应用。 它出现在 Windows 之外无需额外付费并随时可供生产使用。

公平地讲，Windows Admin Center 是在 Windows Server 2019 和 Windows，其他版本运行的单独下载，但它是新建和我们不希望您错过它...

### <a name="storage-migration-service"></a>存储迁移服务

存储迁移服务是一种新技术，可以更轻松地将服务器迁移到更新版本的 Windows Server。 它提供一个图形工具，可清查服务器上的数据、将该数据和配置传输到更新的服务器，然后可以选择将旧服务器的标识移动到新服务器，以便应用和用户不必做出任何更改。 有关详细信息，请参阅[存储迁移服务](storage-migration-service/overview.md)。

### <a id="storage-spaces-direct"></a>存储空间直通 （2019年仅限 Windows Server)

有大量的改进存储空间直通在 Windows Server 2019 （存储空间直通不包括在 Windows Server 半年频道）：

- **重复数据删除和 ReFS 卷的压缩**

    与重复数据删除和 ReFS 文件系统压缩在同一卷上存储最多 10 倍更多数据。 (它具有[只需单击一下](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be)若要使用 Windows Admin Center 打开。)使用可选的压缩大小可变的区块存储可节省率最大化，而多线程的后续处理体系结构保证性能的影响最小。 支持的卷最多 64 TB 和将重复数据删除的每个文件的第一个 4 TB。

- **对永久性内存的本机支持**

    利用针对持久性内存模块的本机存储空间直通支持释放前所未有的性能，包括 Intel® Optane™ DC PM 和 NVDIMM-N。 将持久性内存用作缓存以提高活动工作集的速度，或用作容量以保证一致的微秒数量级低延迟。 就像在 PowerShell 或 Windows Admin Center 中管理任何其他驱动器一样管理持久性内存。

- **两个节点在边缘超聚合基础结构的嵌套复原能力**

    利用从 RAID 5+1 获得启发的全新软件复原选项，一次可承受两个硬件故障。 利用嵌套复原，两个节点的存储空间直通群集可以提供应用和虚拟机的连续访问存储，即使一个服务器节点出现故障并且其他服务器节点中一个驱动器出现故障也可继续工作。

- **两个服务器群集使用 USB 闪存驱动器作为见证服务器**

    使用低成本 USB 闪存驱动器插入到你的路由器充当两个服务器群集中的见证服务器。 如果服务器发生故障，然后重新启动 USB 驱动器群集知道哪些服务器具有最新的数据。 有关详细信息，请参阅[存储在 Microsoft 博客](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/)。

- **Windows Admin Center**

    利用新的[专门构建的仪表板](../manage/windows-admin-center/use/manage-hyper-converged.md)和 Windows Admin Center 中的体验，管理和监视存储空间直通。 只需单击几下，即可创建、打开、展开或删除卷。 监视从整体群集到单个 SSD 或 HDD 的性能，例如 IOPS 和 IO 延迟。 Windows Server 2016 和 Windows Server 2019 上均提供，并且无需支付额外费用。

- **性能历史记录**

    通过[内置历史记录](storage-spaces/performance-history.md)可轻松了解资源利用率和性能。 将自动收集跨越计算、内存、网络和存储的 50 多个基本计数器并将其存储在群集上，存储时间长达一年。 最重要的是，不需要安装、配置或启动，使用非常简单。 在 Windows Admin Center 中可视化或在 PowerShell 中查询和处理。

- **缩放最高支持每个群集 PB**

    实现多 PB 的扩展 – 非常适合介质、备份和存档用例。 在 Windows Server 2019 中，存储空间直通支持每个存储池多达 4 拍字节 (PB) = 4,000 太字节的原始容量。 还增加了相关容量指南：例如，你可以创建两倍多的卷（64 而不是 32），每个都是之前的两倍大（64TB 而不是 32TB）。 将多个群集缝合成[群集设置](storage-spaces/cluster-sets.md)为甚至更大的规模，一个存储命名空间内。 有关详细信息，请参阅[存储在 Microsoft 博客](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/)。

- **镜像加速奇偶校验更快地为 2 X**

    使用镜像加速奇偶校验，你可以创建存储空间直通卷，这些卷一部分是镜像，另一部分是奇偶校验（如混合使用 RAID 1 和 RAID 5/6），从而充分利用两者的优势。 (它具有[比您想象的更容易](https://www.youtube.com/watch?v=R72QHudqWpE)Windows Admin Center 中。)在 Windows Server 2019 镜像加速奇偶校验性能是增加一倍以上相对于 Windows Server 2016 得益于优化。

- **驱动器延迟离群值检测**

    利用从 Microsoft Azure 的长期成功方法获得启发的主动监视和内置异常检测，可轻松识别存在异常延迟的驱动器。 无论是平均延迟还是出现的其他不明显的延迟（如 99% 延迟），低速驱动器会自动在 PowerShell 和 Windows Admin Center 中标记为“异常延迟”状态。

- **手动分隔卷，以提高容错能力的分配**

    这使管理员可以手动分隔的存储空间直通中的卷分配。 这样做以便可显著提高容错能力，某些情况下，但施加了一些增加了的管理注意事项和复杂性。 有关详细信息，请参阅[分隔的卷分配](storage-spaces/delimit-volume-allocation.md)。

### <a name="storage-replica2019"></a>存储副本

有大量的改进[存储副本](storage-replica/storage-replica-overview.md)在此版本中：

#### <a name="storage-replica-in-windows-server-standard-edition"></a>Windows Server，标准版中的存储副本

使用 Windows Server，除了数据中心版的 Standard Edition，现在可以使用存储副本。 在 Windows Server Standard Edition 上运行的存储副本具有以下限制：

- 存储副本复制而不是无限数量的卷的单个卷。
- 卷可以具有最多 2 TB 而不是大小不受限制的大小。

#### <a name="storage-replica-log-performance-improvements"></a>存储副本记录性能改进

我们还改进了存储副本日志跟踪复制的方式提高复制吞吐量和延迟，尤其是在所有闪存存储，以及相互之间复制的存储空间直通群集上。

若要获得更高的性能，复制组的所有成员必须都运行 Windows Server 2019。

#### <a name="test-failover"></a>测试故障转移

现在可以暂时在用于测试的目标服务器上装载的复制存储快照或备份目的。 有关详细信息，请参阅[关于存储副本的常见问题](https://aka.ms/srfaq)。

#### <a name="windows-admin-center-support"></a>Windows Admin Center 支持

对复制的图形管理支持现在 Windows Admin Center 通过服务器管理器工具。 这包括服务器到服务器复制，群集到群集以及拉伸群集复制。

#### <a name="miscellaneous-improvements"></a>其他改进

存储副本还包含以下改进：

-   更改异步拉伸群集行为，因此，现在会发生自动故障转移
-   多个 bug 修复

### <a name="smb"></a>SMB

- **SMB1 和来宾身份验证去除**:默认情况下，Windows Server 可以不再安装 SMB1 客户端和服务器。 此外，SMB2 及更高版本中作为来宾进行身份验证的功能默认情况下处于关闭状态。 有关详细信息，请查看[默认情况下，Windows 10 版本 1709 和 Windows Server 版本 1709 中未安装 SMBv1](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server)。 

- **SMB2/SMB3 安全性和兼容性**:添加了安全和应用程序兼容性的其他选项，包括禁用 oplock SMB2 + 中的旧应用程序，以及需要签名或加密从客户端的每个连接基础上的功能。 有关详细信息，请查看 SMBShare PowerShell 模块帮助。

### <a name="data-deduplication"></a>重复数据删除

- **现在重复数据删除支持 ReFS**:不再必须选择是使用 ReFS 的流行文件系统的优势，重复数据删除： 现在，可以启用重复数据删除，只要可以启用 ReFS。 使用 ReFS 将存储效率提高 95% 以上。
- **优化入口/出口到删除重复卷的系统数据端口 API**:开发人员现在可以利用重复数据删除具有有关如何存储数据，有效地以卷，服务器之间移动数据和有效地群集的知识。

### <a name="file-server-resource-manager"></a>文件服务器资源管理器

在服务启动时，Windows Server 2019 上的所有卷包括的功能，若要阻止的文件服务器资源管理器服务创建变更日志 （也称为 USN 日志）。 这可以节省每个卷的空间，但将禁用实时文件分类。 有关详细信息，请参阅[文件服务器资源管理器概述](fsrm/fsrm-overview.md)。

## <a name="whats-new-in-storage-in-windows-server-version-1803"></a>什么是 Windows Server 版本 1803年中的存储中的新增功能

### <a name="file-server-resource-manager"></a>文件服务器资源管理器

Windows Server 版本 1803年包括的功能可防止文件服务器资源管理器服务创建变更日志 （也称为 USN 日志） 的所有卷在服务启动时。 这可以节省每个卷的空间，但将禁用实时文件分类。 有关详细信息，请参阅[文件服务器资源管理器概述](fsrm/fsrm-overview.md)。

## <a name="whats-new-in-storage-in-windows-server-version-1709"></a>什么是 Windows Server 版本 1709年中的存储中的新增功能

Windows Server 版本 1709年是半年频道中的第一个 Windows Server 版本。 半年频道是软件保障权益，并完全支持在 18 个月内，使用新版本每六个月的生产环境中。

有关详细信息，请参阅 [Windows Server 半年频道概述](../get-started/semi-annual-channel-overview.md)。

### <a name="storage-replica"></a>存储副本

添加存储副本的灾难恢复保护现在扩展以包含：

- **测试故障转移**：现在可以通过测试故障转移功能来选择安装目标存储。 你可以在目标节点上临时安装复制的存储的快照以便进行测试或备份。 有关详细信息，请参阅[关于存储副本的常见问题](https://aka.ms/srfaq)。
- **Windows Admin Center 支持**:对复制的图形管理支持现在 Windows Admin Center 通过服务器管理器工具。 这包括服务器到服务器复制，群集到群集以及拉伸群集复制。

存储副本还包含以下改进：

-   更改异步拉伸群集行为，因此，现在会发生自动故障转移
-   多个 bug 修复

### <a name="smb"></a>SMB

- **SMB1 和来宾身份验证去除**:Windows Server 版本 1709年不再安装 SMB1 客户端和服务器默认情况下。 此外，SMB2 及更高版本中作为来宾进行身份验证的功能默认情况下处于关闭状态。 有关详细信息，请查看[默认情况下，Windows 10 版本 1709 和 Windows Server 版本 1709 中未安装 SMBv1](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server)。 

- **SMB2/SMB3 安全性和兼容性**:添加了安全和应用程序兼容性的其他选项，包括禁用 oplock SMB2 + 中的旧应用程序，以及需要签名或加密从客户端的每个连接基础上的功能。 有关详细信息，请查看 SMBShare PowerShell 模块帮助。

### <a name="data-deduplication"></a>重复数据删除

- **现在重复数据删除支持 ReFS**:不再必须选择是使用 ReFS 的流行文件系统的优势，重复数据删除： 现在，可以启用重复数据删除，只要可以启用 ReFS。 使用 ReFS 将存储效率提高 95% 以上。
- **优化入口/出口到删除重复卷的系统数据端口 API**:开发人员现在可以利用重复数据删除具有有关如何存储数据，有效地以卷，服务器之间移动数据和有效地群集的知识。

## <a name="whats-new-in-storage-in-windows-server-2016"></a>Windows Server 2016 中存储方面的新增功能

### <a name="s2d"></a>存储空间直通  
存储空间直通允许通过使用具有本地存储的服务器构建高可用性和可缩放存储。 该功能简化了软件定义的存储系统的部署和管理并且允许使用 SATA SSD 和 NVMe 磁盘设备等新型磁盘设备，而之前群集存储空间无法使用共享磁盘。  

**这一更改增添了什么价值？**  
空间存储直通使服务提供商和企业可使用带本地存储的行业标准服务器来构建高可用性和高扩展性的软件定义的存储。 使用带本地存储的服务器可降低复杂性、增强可伸缩性，并允许使用之前不可能使用的存储设备，如使用 SATA 固态磁盘降低闪存存储成本或使用 NVMe 固态磁盘实现更加性能。  

空间存储直通不再需要共享 SAS 结构，从而简化了部署和配置。 它改为使用网络作为存储结构，利用 SMB3 和 SMB 直通 (RDMA) 实现高速、低延迟的 CPU 高效存储。 若要横向扩展，只需添加更多服务器以增加存储容量和 I/O 性能  
有关详细信息，请参阅 [Windows Server 2016 中的存储空间直通](storage-spaces/storage-spaces-direct-overview.md).  

**工作原理的不同之处是什么？**  
此功能是 Windows Server 2016 的新增功能。  

### <a name="storage-replica"></a>存储副本

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

### <a name="storage-qos"></a>存储服务质量  
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

### <a name="dedup"></a>重复数据删除  
| 功能 | 新功能或更新功能 | 描述 |
|---------------|----------------|-------------|
| [支持大型卷](data-deduplication/whats-new.md#large-volume-support) | 已更新 | 在 Windows Server 2016 之前，必须专门调整卷的大小实现预期改动，大小超过 10 TB 的卷不适合进行重复数据删除。 在 Windows Server 2016 中，重复数据删除支持**高达 64 TB** 的卷大小。 |
| [支持大型文件](data-deduplication/whats-new.md#large-file-support) | 已更新 | 在 Windows Server 2016 之前，大小接近 1 TB 的文件不适合进行重复数据删除。 在 Windows Server 2016 中，完全支持**高达 1 TB** 的文件。 |
| [Nano Server 的支持](data-deduplication/whats-new.md#nano-server-support) | 新增 | 重复数据删除在 Windows Server 2016 的新 Nano Server 部署选项中可用且完全受支持。 |
| [简化的备份支持](data-deduplication/whats-new.md#simple-backup-support) | 新增 | 在 Windows Server 2012 R2 中，通过一系列手动配置步骤支持虚拟化备份应用程序，如 Microsoft 的 [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx) 在 Windows Server 2016 中，已针对虚拟化备份应用程序的重复数据删除的无缝部署添加了新的默认使用类型“备份”。 |
| [支持群集操作系统滚动升级](data-deduplication/whats-new.md#cluster-upgrade-support) | 新增 | 重复数据删除完全支持 Windows Server 2016 的新的[群集操作系统滚动升级](..//failover-clustering/cluster-operating-system-rolling-upgrade.md)功能。 |

### <a name="smb-hardening-improvements"></a>SMB 强化改进的 SYSVOL 和 NETLOGON 连接  
在到默认的 Active Directory 域服务 SYSVOL 和 NETLOGON 的 Windows 10 和 Windows Server 2016 客户端连接中，域控制器上的共享现在要求 SMB 签名和相互身份验证（例如 Kerberos)。   

**这一更改增添了什么价值？**  
此更改降低了中间人攻击的可能性。   

**工作原理的不同之处是什么？**  
如果 SMB 签名和相互身份验证都不可用，Windows 10 或 Windows Server 2016 计算机不会处理基于域的组策略和脚本。  

> [!NOTE]  
> 这些设置的注册表值默认情况下并不出现，但在被组策略或其他注册表值替代前，强化规则仍然适用。  

有关这些安全改进的详细信息也称为 UNC 强化，请参阅 Microsoft 知识库文章[3000483](https://support.microsoft.com/kb/3000483)和[MS15 011 & MS15-014:强化组策略](https://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy)。  

### <a name="work-folders"></a>工作文件夹
改进了的更改通知时工作文件夹服务器正在运行 Windows Server 2016 和工作文件夹客户端是 Windows 10。

**这一更改增添了什么价值？**<br>
对于 Windows Server 2012 R2，当文件更改同步到工作文件夹服务器上时，不向客户端通知这一更改并等待 10 分钟获取更新。  在使用 Windows Server 2016 时，工作文件夹服务器会立即通知 Windows 10 客户端和立即同步文件更改。

**工作原理的不同之处是什么？**<br>
此功能是 Windows Server 2016 的新增功能。 这需要 Windows Server 2016 工作文件夹服务器和客户端必须是 Windows 10。

如果你使用的是较旧客户端或工作文件夹服务器为 Windows Server 2012 R2，则客户端将继续每 10 分钟轮询一次更改。

### <a name="refs"></a>ReFS 
下一次 ReFS 迭代提供对具有各种工作负荷的大规模存储部署的支持，为你的数据提供可靠性、复原能力和可扩展性。     

**这一更改增添了什么价值？**<br>
ReFS 做了以下改进：

* ReFS 实现了新存储层功能，帮助提供更快的性能和更大的存储容量。 此新功能将启用以下内容：
    * 在同一虚拟磁盘上使用多个复原类型（例如，使用性能层中的镜像和容量层中的奇偶校验）。
    * 提高了对偏离工作集的响应能力。  
* 块克隆的引入大大提高了 VM 操作（例如，.vhdx 检查点合并操作）的性能。
* 新的 ReFS 扫描工具支持泄露存储的恢复，并可帮助回收数据避免严重损坏。 

**工作原理的不同之处是什么？**<br>
这些功能是 Windows Server 2016 中的新增功能。 

## <a name="see-also"></a>请参阅  
* [Windows Server 2016 中的新增功能](../get-started/what-s-new-in-windows-server-2016.md)  
