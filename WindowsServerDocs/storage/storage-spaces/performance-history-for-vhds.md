---
title: 虚拟硬盘的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880398"
---
# <a name="performance-history-for-virtual-hard-disks"></a>虚拟硬盘的性能历史记录

> 适用于：Windows Server Insider Preview

此子主题[的存储空间直通的性能历史记录](performance-history.md)详细地介绍了为虚拟硬盘 (VHD) 文件收集的性能历史记录。 性能历史记录是可用于每个 VHD 附加到正在运行的群集虚拟机。 性能历史记录为适用于 VHD 和 VHDX 格式，但它不是适用于共享 VHDX 文件。

   > [!NOTE]
   > 可能需要几分钟时间集合以开始为新创建或移动 VHD 文件。

## <a name="series-names-and-units"></a>序列名称和单位

这些系列将收集有关每个符合条件的虚拟硬盘：

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
| `vhd.iops.read`           | 每秒完成的虚拟硬盘的读取操作的次数。                                         |
| `vhd.iops.write`          | 每秒完成的虚拟硬盘的写入操作数目。                                        |
| `vhd.iops.total`          | 总数的读取或写入操作每秒完成的虚拟硬盘。                          |
| `vhd.throughput.read`     | 从虚拟硬盘，每秒读取的数据的数量。                                                     |
| `vhd.throughput.write`    | 写入到磁盘的虚拟硬盘每秒的数据的数量。                                                    |
| `vhd.throughput.total`    | 数据读取或写入到磁盘的虚拟硬盘每秒的总数量。                                 |
| `vhd.latency.average`     | 到或从虚拟硬盘的所有操作的平均延迟。                                              |
| `vhd.size.current`        | 如果动态扩展虚拟硬盘磁盘的当前文件大小。 如果固定的则不会收集该系列。 |
| `vhd.size.maximum`        | 如果动态扩展虚拟硬盘磁盘最大大小。 如果固定的是的大小。                  |

## <a name="where-they-come-from"></a>这些来自

`iops.*`， `throughput.*`，并`latency.*`系列收集从`Hyper-V Virtual Storage Device`性能计数器虚拟机的位置运行，每个 VHD 或 VHDX 的一个实例，请在服务器上设置。

| 系列                    | 源计数器         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *上述的总和*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *上述的总和*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > 计数器来度量在整个间隔内，不会被取样。 例如，如果处于非活动状态的 VHD 是 9 秒但在完成时 30 IOs 10，10 秒，其`vhd.iops.total`记录为 3 个 IOs 每秒平均此 10 秒间隔内。 这可确保其性能历史记录捕获所有活动也很可靠到干扰。

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用[GET-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) cmdlet:

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

若要从虚拟机中获取每个 VHD 的路径：

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > GET-VHD cmdlet 需要一个文件路径来提供。 它不支持枚举。

## <a name="see-also"></a>请参阅

- [有关存储空间直通的性能历史记录](performance-history.md)
