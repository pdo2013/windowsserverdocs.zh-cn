---
title: 存储空间直通-方面的常见问题
description: 了解如何存储空间直通
keywords: 存储空间
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818908"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>存储空间直通-常见问题 (FAQ)

本文列出了一些常见，并且与相关的常见问题[存储空间直通](storage-spaces-direct-overview.md)。

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>当您使用存储空间直通使用 3 个节点时，您可以获得性能和容量层？

是的可以在节点 2 或 3 个节点的存储空间直通配置中获取的性能和容量层。 但是，必须确保有 2 个容量设备。 这意味着您必须使用所有三种类型的设备：NVME、 SSD 和 HDD。
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Refs 文件系统提供了实时 tiaring 使用存储空间直通。 没有 REFS 提供相同的功能使用共享的存储空间在 2016年？

否，不会获得实时与 2016年具有共享的存储空间分层。 这是用于存储空间直通。 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>可以使用存储空间直通使用 NTFS 文件系统？
  
是的您可以使用存储空间直通使用 NTFS 文件系统。 但是，建议 REFS。 NTFS 不提供实时分层。 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>我已配置 2 个节点存储空间直通群集，其中的虚拟磁盘配置为双向镜像复原能力。 我将添加一个新的容错域，将更改现有的虚拟磁盘的复原能力？

添加新的容错域后，新创建的虚拟磁盘将跳转至三向镜像。 但是，现有的虚拟磁盘将保持的双向镜像的磁盘。 从现有卷，以获得新的复原能力的优点，可以将数据复制到新的虚拟磁盘。
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>存储空间直通是使用创建 autoconfig:0开关和手动创建的池。 当我尝试查询要创建新的卷的存储空间直通池时，我收到消息，指出:"Enable-ClusterS2D 再次。"我应该做什么？

默认情况下，使用启用 S2D cmdlet 配置存储空间直通时该 cmdlet 的所有内容为您完成。 它会创建池和层级。 在使用 autoconfig:0，必须手动完成的所有内容。 如果创建仅池时，不一定被创建层。 如果将根本不创建层，或者未附加的设备相对应的方式创建层，将收到"再次 Enable-ClusterS2D"错误消息。 我们建议不要在生产环境中使用自动配置开关。 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>是否可以使用 SSD 设备具有创建存储空间直通后将旋转磁盘 (HDD) 添加到存储空间直通池？

否。 默认情况下，如果使用单个设备类型以创建池，它不会配置缓存磁盘和所有磁盘都用于容量。 可以将 NVME 磁盘添加到配置中，和 NVME 磁盘将为缓存配置。
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>我已配置 2 机架容错域：机架 1 具有 2 个容错域，机架 2 具有 1 个容错域。 每个服务器有 4 个容量 100GB 设备。 可以从池中使用所有 1200 GB 的空间？

不可以，你可以使用仅 800 GB。 在机架容错域中，您必须确保，必须使每个删除的双向镜像配置和其重复 land 不同机架中。
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>缓存大小时应该是什么我配置的存储空间直通？

缓存应将其调整为适应工作集 (正在主动的数据读取或写入在任何给定时间) 的应用程序和工作负荷。

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>如何确定正在使用存储空间直通的缓存的大小？

使用 PerfMon 的内置实用程序来检查缓存未命中数。 查看缓存未命中每秒读取从群集存储混合磁盘计数器。 请记住是否太多读取缺少缓存，你的缓存是较小，可能想要将其展开。 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>有一个显示正在将留出缓存、 容量和复原能力，从而使我能够更好地计划的磁盘的确切大小的计算器吗？

可以使用存储空间计算器来帮助进行规划。 该查看器 http://aka.ms/s2dcalc。
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>什么是配置 6 台和 3 个机架时建议的最佳配置？

每个机架上使用 2 个服务器，若要获取的三向镜像虚拟磁盘复原能力。 请记住，当机架配置仅当提供方式放置在机架上的操作系统的配置，将正常工作。 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>可以启用存储空间直通群集中的特定服务器上的特定磁盘的维护模式？

是的可以启用特定磁盘和一个特定的容错域上的存储维护模式。 当你暂停一个节点时自动调用启用 StorageMaintenanceMode 命令。 您可以为特定磁盘启用它通过运行以下命令：

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>存储空间直通支持我的硬件？

我们建议联系硬件供应商联系以验证支持。 硬件供应商测试其硬件和关于是否支持或未注释的解决方案。 例如，在此撰写本文时，服务器，例如 R730 / R730xd / R630 有 8 个以上的驱动器插槽可支持 SES 和与存储空间直通兼容。 Dell 支持仅使用存储空间直通 HBA330。 R620 不支持 SES 和与存储空间直通不兼容。

有关更多的硬件的支持信息，请转到以下网站：Windows Server 编录
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>如何存储空间直通做以后，使用的 SES？

存储空间直通使用 SCSI 机箱服务 (SES) 映射以确保数据和元数据的 slabs 跨多个以弹性方式的容错域。 如果硬件不支持 SES，机箱，没有映射和数据放置不弹性。
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>您可以使用哪些命令来检查虚拟磁盘的物理范围？
  
这个：

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
