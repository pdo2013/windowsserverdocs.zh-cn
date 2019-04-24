---
title: 针对存储空间直通的性能优化
description: 存储空间直通自动根据你使用的硬件的缓存配置优化其性能，如本主题所述。
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.assetid: 15a519fa-37cc-4d84-a9fe-097d33bb71ea
author: phstee
ms.author: Vshankar; DanLo; clausjor; StevenEk
ms.date: 4/14/2017
ms.openlocfilehash: 280d0e298afe5c9628fe73872e0983f819f2a3b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891298"
---
# <a name="performance-tuning-for-storage-spaces-direct"></a>针对存储空间直通的性能优化

存储空间直通是基于 Windows Server 的软件定义的存储解决方案，可以自动优化其性能，不需手动指定列计数、所用硬件的缓存配置，以及其他必须使用共享 SAS 存储解决方案手动进行设置的因素。 有关背景信息，请参阅 [Windows Server 2016 中的存储空间直通](../../../../storage/storage-spaces/storage-spaces-direct-overview.md)。

存储空间直通软件存储总线缓存自动根据系统中存在的存储的类型进行配置。 识别三种类型：**HDD**、**SSD**、**NVMe**。 缓存会根据需要声明最快速的存储进行读取和/或写入缓存操作，将较慢的存储用于数据的持久存储。

下表汇总了默认设置：

| 存储类型 | 缓存配置 |
| --- | --- |
| 任意单一类型 | 如果只存在一种存储类型，则不配置软件存储总线缓存。 |
| SSD+HDD 或 NVMe+HDD | 最快的存储配置为缓存层，缓存读取和写入。 |
| SSD+SSD 或 NVMe+NVMe | 这些快+快选项的目标是组合使用耐用性较高和较低的存储。例如，将每日驱动器写入次数 (DWPD) 为 10 的 NAND 闪存 SSD 用于缓存，将 DWPD 为 1.5 的 NAND 闪存 SSD 用于容量。 其启用方式是为存储空间直通提供一组用于标识缓存设备的模型字符串。 有关详细信息，请查看 [Enable-StorageSpacesDirect](https://technet.microsoft.com/library/mt589697.aspx) cmdlet 参考 (`CacheDeviceModel`)。 <br><br>在快+快系统中，只缓存写入， 不缓存读取。 |

请注意，基于 SSD 或 NVMe 设备的缓存默认为只进行写入缓存。 这里的考虑是，由于容量设备为快速设备，将读取内容移到缓存设备的价值不大。 有时候这种考虑也许是多余的，但仍需小心谨慎，因为启用读取缓存可能会不必要地导致缓存设备的耐用性降低，而性能却没有任何提高。 例如：

* **NVme+SSD** 启用读取缓存可以让读取 IO 充分利用 PCIe 连接性和/或 NVMe 设备的更高 IOPS 性能（相对于聚合 SSD 来说）。 <br>考虑到 NVMe 设备相对于连接到 SSD 的 HBA 的带宽能力，这可能适用于以带宽为导向的方案。 这可能不适用于以 IOPS 为导向的方案，此类方案在实现性能提高之前就可能会因为 IOPS 的 CPU 成本而导致系统受到限制。
* **NVMe+NVMe** 同样，如果缓存 NVMe 的读取能力强于组合的容量 NVMe，也许可以启用读取缓存。 <br>正常情况下，适合在这些配置中启用读取缓存的例子并不常见。

若要查看和更改缓存配置，请使用 [Get-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt634616.aspx) 和 [Set-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt763265.aspx) cmdlet。 `CacheModeHDD` 和 `CacheModeSSD` 属性定义缓存在所指示类型的容量介质上的运行方式。

## <a name="see-also"></a>另请参阅

- [了解存储空间直通](../../../../storage/storage-spaces/understand-storage-spaces-direct.md)
- [规划存储空间直通](../../../../storage/storage-spaces/plan-storage-spaces-direct.md)
- [文件服务器的性能优化](../../role/file-server/index.md)
- [Software-Defined Storage Design Considerations Guide](https://technet.microsoft.com/library/mt243829.aspx)（软件定义的存储设计注意事项指南）（适用于 Windows Server 2012 R2 和共享 SAS 存储）
