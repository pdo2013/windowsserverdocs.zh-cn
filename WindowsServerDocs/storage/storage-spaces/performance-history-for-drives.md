---
title: 对驱动器的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879188"
---
# <a name="performance-history-for-drives"></a>对驱动器的性能历史记录

> 适用于：Windows Server Insider Preview

此子主题[的存储空间直通的性能历史记录](performance-history.md)详细地介绍了为驱动器收集的性能历史记录。 性能历史记录可用于每个驱动器在群集存储子系统中，而不考虑总线或媒体类型。 但是，不用于 OS 启动驱动器。

   > [!NOTE]
   > 不能为驱动器已关闭的服务器中收集性能历史记录。 当服务器重新启动后，集合将自动恢复。

## <a name="series-names-and-units"></a>序列名称和单位

这些系列将收集有关每个符合条件的驱动器：

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
| `physicaldisk.iops.read`        | 每秒完成的驱动器的读取操作的次数。                |
| `physicaldisk.iops.write`       | 每秒完成的驱动器的写入操作数目。               |
| `physicaldisk.iops.total`       | 总数的读取或写入操作每秒完成的驱动器。 |
| `physicaldisk.throughput.read`  | 从驱动器每秒读取的数据的数量。                            |
| `physicaldisk.throughput.write` | 数据写入到每秒的驱动器数量。                           |
| `physicaldisk.throughput.total` | 数据读取或写入到每秒的驱动器的总数量。        |
| `physicaldisk.latency.read`     | 从驱动器读取操作的平均延迟。                          |
| `physicaldisk.latency.write`    | 对驱动器进行写入操作的平均延迟。                           |
| `physicaldisk.latency.average`  | 到或从驱动器的所有操作的平均延迟。                     |
| `physicaldisk.size.total`       | 驱动器的总存储容量。                                    |
| `physicaldisk.size.used`        | 驱动器已用存储空间容量。                                     |

## <a name="where-they-come-from"></a>这些来自

`iops.*`， `throughput.*`，并`latency.*`系列收集从`Physical Disk`在服务器上设置该驱动器已连接的性能计数器，每个驱动器的一个实例。 这些计数器测量由`partmgr.sys`，但不包括的 Windows 软件堆栈和任何网络跃点。 它们是代表设备硬件性能。

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
   > 计数器来度量在整个间隔内，不会被取样。 例如，如果在驱动器处于空闲状态 9 秒但在完成时 30 IOs 10，10 秒，其`physicaldisk.iops.total`记录为 3 个 IOs 每秒平均此 10 秒间隔内。 这可确保其性能历史记录捕获所有活动也很可靠到干扰。

`size.*`系列收集从`MSFT_PhysicalDisk`类在 WMI 中，每个驱动器的一个实例。

| 系列                          | Source 属性        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用[Get-physicaldisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) cmdlet:

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>请参阅

- [有关存储空间直通的性能历史记录](performance-history.md)
