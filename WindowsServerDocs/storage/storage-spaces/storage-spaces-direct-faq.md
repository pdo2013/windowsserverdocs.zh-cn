---
title: 存储空间直通-常见问题
description: 了解存储空间直通
keywords: 存储空间
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 1aafac9a994e907e8b8ee3b556618d630cdf8418
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872074"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>存储空间直通常见问题（FAQ）

本文列出了与[存储空间直通](storage-spaces-direct-overview.md)相关的一些常见问题和常见问题。

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>当你使用包含3个节点的存储空间直通时，是否可以获得性能和容量级别？

是的，你可以在两节点存储空间直通配置中获取性能和容量层。 但是，必须确保具有2个容量设备。 这意味着，必须使用所有这三种类型的设备：NVME、SSD 和 HDD。
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Refs 文件系统提供存储空间直通的实时 tiaring。 REFS 在2016中提供与共享存储空间相同的功能吗？

不可以，你将不会获得带有2016的共享存储空间的实时分层。 这仅适用于存储空间直通。 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>是否可以将 NTFS 文件系统用于存储空间直通？
  
是的，可以将 NTFS 文件系统用于存储空间直通。 但建议使用 REFS。 NTFS 不提供实时分层。 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>我配置了2个节点存储空间直通群集，其中虚拟磁盘配置为双向镜像复原。 如果添加新的容错域，现有虚拟磁盘的复原是否会改变？

添加新的容错域后，你创建的新虚拟磁盘将跳转到3向镜像。 但是，现有虚拟磁盘将保留为双向镜像磁盘。 可以从现有卷将数据复制到新的虚拟磁盘，以获得新的复原能力。
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>该存储空间直通是使用自动配置：0创建的开关和手动创建的池。 当我尝试查询存储空间直通池来创建新卷时，会收到一条消息，显示 "Enable-clusters2d"。我该怎么办？

默认情况下，当你使用 enable-S2D cmdlet 配置存储空间直通时，该 cmdlet 会为你执行所有操作。 它创建池和层。 使用自动配置时，必须手动完成所有操作。 如果只创建了池，则不一定要创建层。 如果未使用与附加的设备相对应的方式在所有层或未创建层上创建层，将会收到 "Enable-clusters2d" 错误消息。 建议你不要在生产环境中使用自动配置开关。 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>使用 SSD 设备创建存储空间直通后，是否可以向存储空间直通池添加旋转磁盘（HDD）？

否。 默认情况下，如果使用单个设备类型创建池，则它不会配置缓存磁盘，所有磁盘都将用于容量。 可以将 NVME 磁盘添加到配置中，并为缓存配置 NVME 磁盘。
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>我配置了一个2机架的容错域：机架1具有2个容错域，第2机架有1个容错域。 每台服务器有4个容量 100 GB 设备。 能否使用池中的所有 1200 GB 空间？

不能，只能使用 800 GB。 在机架容错域中，你必须确保有一个双向镜像配置，使每个 chuck 及其在不同机架中的重复。
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>配置存储空间直通时，缓存大小是多少？

应该调整缓存的大小，以适应你的应用程序和工作负荷的工作集（在任意给定时间主动读取或写入的数据）。

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>如何确定存储空间直通正在使用的缓存大小？

使用内置的实用工具 PerfMon 检查缓存未命中。 查看群集存储混合磁盘计数器中的缓存未命中读取数/秒。 请记住，如果有太多读取丢失缓存，则您的缓存会很小，您可能希望将其展开。 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>是否有计算器显示为缓存、容量和复原而保留的磁盘的准确大小，这使我可以更好地进行规划呢？

可以使用存储空间计算器来帮助进行规划。 可从 http://aka.ms/s2dcalc 获取。
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>配置6台服务器和3个机架时，建议的最佳配置是什么？

请在每个机架上使用2个服务器，以获取一个三向镜像的虚拟磁盘复原能力。 请记住，仅当在机架上放置操作系统时，机架配置才能正常工作。 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>能否为存储空间直通群集中特定服务器上的特定磁盘启用维护模式？

是的，你可以在特定磁盘和特定容错域上启用存储维护模式。 暂停节点时，将自动调用 StorageMaintenanceMode 命令。 可以通过运行以下命令为特定磁盘启用它：

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>硬件上是否支持存储空间直通？

建议你与硬件供应商联系以验证支持。 硬件供应商在其硬件上测试解决方案，并注释是否支持。 例如，在撰写本文时，具有8个以上驱动器插槽的服务器（例如 R730/R730xd/R630）可支持 SES，并与存储空间直通兼容。 Dell 仅支持存储空间直通的 HBA330。 R620 不支持 SES，与存储空间直通不兼容。

有关硬件支持的详细信息，请参阅以下网站：Windows Server 编录
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>存储空间直通如何利用 SES？

存储空间直通使用 SCSI 机箱服务（SES）映射，确保数据和元数据的碎片以弹性方式分散到容错域。 如果硬件不支持 SES，则不会映射机箱，并且数据位置不能复原。
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>用于检查虚拟磁盘的物理范围的命令有哪些？
  
这个：

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
