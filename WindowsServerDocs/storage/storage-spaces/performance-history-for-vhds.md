---
title: 虚拟硬盘性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589715"
---
# <a name="performance-history-for-virtual-hard-disks"></a>虚拟硬盘性能历史记录

> 应用于： Windows Server 内幕预览

虚拟硬盘 (VHD) 文件中收集的性能历史记录的详细介绍了[性能历史记录存储空格直接](performance-history.md)此子主题。 适用于每个附加到运行、 群集虚拟机的 VHD 性能历史记录。 性能历史记录是适用于 VHD 和 VHDX 格式，但是不适用于共享 VHDX 文件。

   > [!NOTE]
   > 可能需要几分钟，以便开始新创建的或移动的 VHD 文件的集合。

## <a name="series-names-and-units"></a>系列名称和单位

为每个合格的虚拟硬盘收集这些系列：

| 系列                    | 单位             |
|---------------------------|------------------|
| `vhd.iops.read`           | 每秒       |
| `vhd.iops.write`          | 每秒       |
| `vhd.iops.total`          | 每秒       |
| `vhd.throughput.read`     | 每秒字节数 |
| `vhd.throughput.write`    | 每秒字节数 |
| `vhd.throughput.total`    | 每秒字节数 |
| `vhd.latency.average`     | 秒          |
| `vhd.size.current`        | 字节数            |
| `vhd.size.maximum`        | 字节数            |

## <a name="how-to-interpret"></a>如何解释

| 系列                    | 如何解释                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | 每秒由虚拟硬盘完成的读取操作的数量。                                         |
| `vhd.iops.write`          | 每秒由虚拟硬盘完成的写操作的数量。                                        |
| `vhd.iops.total`          | 读取或写入每秒由虚拟硬盘完成的操作的总数。                          |
| `vhd.throughput.read`     | 从虚拟硬盘每秒读取的数据的数量。                                                     |
| `vhd.throughput.write`    | 数据写入虚拟硬盘每秒的数量。                                                    |
| `vhd.throughput.total`    | 数据读取或写入虚拟硬盘每秒总数量。                                 |
| `vhd.latency.average`     | 虚拟硬盘中的所有操作的平均延迟。                                              |
| `vhd.size.current`        | 虚拟硬盘，如果动态扩展当前文件大小。 如果固定的不会收集数据系列。 |
| `vhd.size.maximum`        | 虚拟硬盘，如果动态扩展最大大小。 如果固定的大小。                  |

## <a name="where-they-come-from"></a>它们来自

`iops.*`， `throughput.*`，和`latency.*`系列收集从`Hyper-V Virtual Storage Device`性能计数器设置在服务器上虚拟机其中是运行，每个 VHD 或 VHDX 的一个实例。

| 系列                    | 源计数器         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *以上的总和*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *以上的总和*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > 计数器通过整个间隔，不会被取样开始算起。 例如，如果 VHD 处于非活动状态的 9 秒但完成 30 IOs 在 10 秒，其`vhd.iops.total`将记录为 3 IOs 每秒平均此 10 秒间隔内。 这样可确保其性能历史记录捕获所有活动和可靠到干扰。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用情况

使用[Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) cmdlet:

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

若要从虚拟机中获取的每个 VHD 路径：

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > Get VHD cmdlet 需要提供文件路径。 它不支持枚举。

## <a name="see-also"></a>另请参阅

- [性能的存储空间直接的历史记录](performance-history.md)
