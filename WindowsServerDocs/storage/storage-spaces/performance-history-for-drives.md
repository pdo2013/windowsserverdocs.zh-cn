---
title: 驱动器性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589749"
---
# <a name="performance-history-for-drives"></a>驱动器性能历史记录

> 应用于： Windows Server 内幕预览

[性能历史记录存储空格直接](performance-history.md)此子主题介绍为驱动器收集的性能历史记录的详细信息。 性能历史记录了可供群集存储子系统，无论总线中每个驱动器或媒体类型。 但是，不适用于 OS 启动驱动器。

   > [!NOTE]
   > 无法为已关闭的服务器中的驱动器收集性能历史记录。 当服务器重新启动后，集合将自动恢复。

## <a name="series-names-and-units"></a>系列名称和单位

为每个符合条件的驱动器收集这些系列：

| 系列                          | 单位             |
|---------------------------------|------------------|
| `physicaldisk.iops.read`        | 每秒       |
| `physicaldisk.iops.write`       | 每秒       |
| `physicaldisk.iops.total`       | 每秒       |
| `physicaldisk.throughput.read`  | 每秒字节数 |
| `physicaldisk.throughput.write` | 每秒字节数 |
| `physicaldisk.throughput.total` | 每秒字节数 |
| `physicaldisk.latency.read`     | 秒          |
| `physicaldisk.latency.write`    | 秒          |
| `physicaldisk.latency.average`  | 秒          |
| `physicaldisk.size.total`       | 字节数            |
| `physicaldisk.size.used`        | 字节数            |

## <a name="how-to-interpret"></a>如何解释

| 系列                          | 如何解释                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | 每秒由驱动器已完成的读取操作的数量。                |
| `physicaldisk.iops.write`       | 每秒由驱动器已完成的写操作的数量。               |
| `physicaldisk.iops.total`       | 读取或写入每秒由驱动器已完成的操作的总数。 |
| `physicaldisk.throughput.read`  | 从每秒的驱动器读取的数据的数量。                            |
| `physicaldisk.throughput.write` | 写入每秒的驱动器的数据的数量。                           |
| `physicaldisk.throughput.total` | 读取或写入每秒的驱动器的数据的总量。        |
| `physicaldisk.latency.read`     | 从驱动器的读取操作的平均延迟。                          |
| `physicaldisk.latency.write`    | 到驱动器写入操作的平均延迟。                           |
| `physicaldisk.latency.average`  | 所有操作或从驱动器的平均延迟。                     |
| `physicaldisk.size.total`       | 驱动器的总存储容量。                                    |
| `physicaldisk.size.used`        | 驱动器的使用的存储容量。                                     |

## <a name="where-they-come-from"></a>它们来自

`iops.*`， `throughput.*`，和`latency.*`系列收集从`Physical Disk`服务器上设置连接驱动器的位置的性能计数器，每个驱动器的一个实例。 这些计数器的衡量`partmgr.sys`，不包括多个 Windows 软件堆栈也任何网络跃点数。 它们是代表设备硬件性能。

| 系列                          | 源计数器           |
|---------------------------------|--------------------------|
| `physicaldisk.iops.read`        | `Disk Reads/sec`         |
| `physicaldisk.iops.write`       | `Disk Writes/sec`        |
| `physicaldisk.iops.total`       | `Disk Transfers/sec`     |
| `physicaldisk.throughput.read`  | `Disk Read Bytes/sec`    |
| `physicaldisk.throughput.write` | `Disk Write Bytes/sec`   |
| `physicaldisk.throughput.total` | `Disk Bytes/sec`         |
| `physicaldisk.latency.read`     | `Avg. Disk sec/Read`     |
| `physicaldisk.latency.write`    | `Avg. Disk sec/Writes`   |
| `physicaldisk.latency.average`  | `Avg. Disk sec/Transfer` |

   > [!NOTE]
   > 计数器通过整个间隔，不会被取样开始算起。 例如，如果驱动器为空闲 9 秒但完成 30 IOs 在 10 秒，其`physicaldisk.iops.total`将记录为 3 IOs 每秒平均此 10 秒间隔内。 这样可确保其性能历史记录捕获所有活动和可靠到干扰。

`size.*`系列收集从`MSFT_PhysicalDisk`类在 WMI 中，每个驱动器的一个实例。

| 系列                          | Source 属性        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用情况

使用[Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) cmdlet:

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>另请参阅

- [性能的存储空间直接的历史记录](performance-history.md)
