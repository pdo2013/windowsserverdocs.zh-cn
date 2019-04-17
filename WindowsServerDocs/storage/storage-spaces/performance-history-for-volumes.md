---
title: 卷性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589698"
---
# <a name="performance-history-for-volumes"></a>卷性能历史记录

> 应用于： Windows Server 内幕预览

[性能历史记录存储空格直接](performance-history.md)此子主题介绍为卷收集的性能历史记录的详细信息。 可用的每个群集共享卷 (CSV) 群集中性能历史记录。 但是，不适用于 OS 启动卷和任何其他非 CSV 存储。

   > [!NOTE]
   > 可能需要几分钟，以便开始的新创建或重命名卷的集合。

## <a name="series-names-and-units"></a>系列名称和单位

每个合格的卷会收集这些系列：

| 系列                    | 单位             |
|---------------------------|------------------|
| `volume.iops.read`        | 每秒       |
| `volume.iops.write`       | 每秒       |
| `volume.iops.total`       | 每秒       |
| `volume.throughput.read`  | 每秒字节数 |
| `volume.throughput.write` | 每秒字节数 |
| `volume.throughput.total` | 每秒字节数 |
| `volume.latency.read`     | 秒          |
| `volume.latency.write`    | 秒          |
| `volume.latency.average`  | 秒          |
| `volume.size.total`       | 字节数            |
| `volume.size.available`   | 字节数            |

## <a name="how-to-interpret"></a>如何解释

| 系列                    | 如何解释                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | 每秒由此卷完成的读取操作的数量。                |
| `volume.iops.write`       | 每秒由此卷完成的写操作的数量。               |
| `volume.iops.total`       | 读取或写入每秒由此卷完成的操作的总数。 |
| `volume.throughput.read`  | 从这个卷每秒读取的数据的数量。                            |
| `volume.throughput.write` | 数据写入此卷每秒的数量。                           |
| `volume.throughput.total` | 数据读取或写入到此卷每秒总数量。        |
| `volume.latency.read`     | 从该卷的读取操作的平均延迟。                          |
| `volume.latency.write`    | 为此批量写入操作的平均延迟。                           |
| `volume.latency.average`  | 所有操作或从该卷的平均延迟。                     |
| `volume.size.total`       | 卷的总存储容量。                                     |
| `volume.size.available`   | 卷的可用存储容量。                                 |

## <a name="where-they-come-from"></a>它们来自

`iops.*`， `throughput.*`，和`latency.*`系列收集从`Cluster CSVFS`性能计数器设置。 群集中的每台服务器具有每个 CSV 音量，无论所有权的实例。 性能历史记录的卷`MyVolume`的聚合`MyVolume`群集中的每台服务器上的实例。

| 系列                    | 源计数器         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *以上的总和*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *以上的总和*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *以上的平均值* |

   > [!NOTE]
   > 计数器通过整个间隔，不会被取样开始算起。 例如，如果卷是空闲 9 秒但完成 30 IOs 在 10 秒，其`volume.iops.total`将记录为 3 IOs 每秒平均此 10 秒间隔内。 这样可确保其性能历史记录捕获所有活动和可靠到干扰。

   > [!TIP]
   > 这些是使用的常用[VM 五星](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1)基准框架的同一个计数器。

`size.*`系列收集从`MSFT_Volume`类在 WMI 中，每个卷的一个实例。

| 系列                    | Source 属性 |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用情况

使用[Get-音量](https://docs.microsoft.com/powershell/module/storage/get-volume)cmdlet:

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>另请参阅

- [性能的存储空间直接的历史记录](performance-history.md)
