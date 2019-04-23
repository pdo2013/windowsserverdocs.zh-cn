---
title: 卷的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882948"
---
# <a name="performance-history-for-volumes"></a>卷的性能历史记录

> 适用于：Windows Server Insider Preview

此子主题[的存储空间直通的性能历史记录](performance-history.md)详细地介绍了为卷收集的性能历史记录。 性能历史记录是为每个群集共享卷 (CSV) 群集中可用。 但是，不用于 OS 引导卷和任何其他非 CSV 存储。

   > [!NOTE]
   > 可能需要几分钟时间集合以开始对新创建的或已重命名卷。

## <a name="series-names-and-units"></a>序列名称和单位

这些系列将收集有关每个符合条件的卷：

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
| `volume.iops.read`        | 每秒完成的此卷的读取操作的次数。                |
| `volume.iops.write`       | 每秒完成的此卷的写入操作数目。               |
| `volume.iops.total`       | 读取或每秒完成此卷的写入操作数的总数。 |
| `volume.throughput.read`  | 从该卷每秒读取的数据的数量。                            |
| `volume.throughput.write` | 为此卷每秒写入的数据的数量。                           |
| `volume.throughput.total` | 数据读取或写入此卷每秒的总数量。        |
| `volume.latency.read`     | 请从该卷的读取操作的平均延迟。                          |
| `volume.latency.write`    | 为此卷的写入操作的平均延迟。                           |
| `volume.latency.average`  | 与此卷的所有操作的平均延迟。                     |
| `volume.size.total`       | 卷的总存储容量。                                     |
| `volume.size.available`   | 卷的可用存储容量。                                 |

## <a name="where-they-come-from"></a>这些来自

`iops.*`， `throughput.*`，并`latency.*`系列收集从`Cluster CSVFS`性能计数器集。 在群集中的每个服务器具有每个 CSV 卷，而不考虑所有权的实例。 为卷记录的性能历史记录`MyVolume`的聚合`MyVolume`群集中每个服务器上的实例。

| 系列                    | 源计数器         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *上述的总和*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *上述的总和*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *上述平均值* |

   > [!NOTE]
   > 计数器来度量在整个间隔内，不会被取样。 例如，如果卷处于空闲状态 9 秒但在完成时 30 IOs 10，10 秒，其`volume.iops.total`记录为 3 个 IOs 每秒平均此 10 秒间隔内。 这可确保其性能历史记录捕获所有活动也很可靠到干扰。

   > [!TIP]
   > 这些是使用流行的相同计数器[VM Fleet](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1)基准测试框架。

`size.*`系列收集从`MSFT_Volume`类在 WMI 中，每个卷的一个实例。

| 系列                    | Source 属性 |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用[Get-volume](https://docs.microsoft.com/powershell/module/storage/get-volume) cmdlet:

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>请参阅

- [有关存储空间直通的性能历史记录](performance-history.md)
