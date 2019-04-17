---
title: 服务器性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894246"
---
# <a name="performance-history-for-servers"></a>服务器性能历史记录

> 应用于： Windows Server 内幕预览

[性能历史记录存储空格直接](performance-history.md)此子主题介绍服务器收集的性能历史记录的详细信息。 性能历史记录仅供群集中的每台服务器。

   > [!NOTE]
   > 无法为已关闭的服务器收集性能历史记录。 当服务器重新启动后，集合将自动恢复。

## <a name="series-names-and-units"></a>系列名称和单位

合格的每台服务器会收集这些系列：

| 系列                           | 单位    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | % |
| `clusternode.cpu.usage.guest`    | % |
| `clusternode.cpu.usage.host`     | % |
| `clusternode.memory.total`       | 字节数   |
| `clusternode.memory.available`   | 字节数   |
| `clusternode.memory.usage`       | 字节数   |
| `clusternode.memory.usage.guest` | 字节数   |
| `clusternode.memory.usage.host`  | 字节数   |

此外，如驱动器系列`physicaldisk.size.total`附加到服务器，并如网络适配器系列的所有合格驱动器聚合`networkadapter.bytes.total`附加到服务器的所有合格的网络适配器的聚合。

## <a name="how-to-interpret"></a>如何解释

| 系列                           | 如何解释                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | 不处于空闲状态的处理器时间的百分比。                        |
| `clusternode.cpu.usage.guest`    | 用于来宾 （虚拟机） 需求的处理器时间的百分比。 |
| `clusternode.cpu.usage.host`     | 用于主机需求的处理器时间的百分比。                    |
| `clusternode.memory.total`       | 服务器的物理内存总量。                              |
| `clusternode.memory.available`   | 服务器的可用内存。                                   |
| `clusternode.memory.usage`       | 服务器的分配 （不可用） 的内存。                   |
| `clusternode.memory.usage.guest` | 分配给来宾 （虚拟机） 需求的内存。               |
| `clusternode.memory.usage.host`  | 分配给主机需求的内存。                                  |

## <a name="where-they-come-from"></a>它们来自

`cpu.*`从不同的性能计数器，具体取决于是否启用 HYPER-V 收集系列。

如果启用 HYPER-V:

| 系列                           | 源计数器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

使用`% Total Run Time`计数器可确保性能历史记录属性所有使用率。

如果未启用 HYPER-V:

| 系列                           | 源计数器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *零* |
| `clusternode.cpu.usage.host`     | *总使用率相同* |

尽管不完美同步`clusternode.cpu.usage`始终`clusternode.cpu.usage.host`和`clusternode.cpu.usage.guest`。

使用相同时应注意，`clusternode.cpu.usage.guest`是始终的总和`vm.cpu.usage`的主机服务器上的所有虚拟机。

`memory.*` （即将推出） 的系列。

  > [!NOTE]
  > 计数器通过整个间隔，不会被取样开始算起。 例如，如果服务器的空闲时间 9 秒到 100%的 CPU 的高峰在 10 秒，其`clusternode.cpu.usage`将记录为 10%平均此 10 秒间隔内。 这样可确保其性能历史记录捕获所有活动和可靠到干扰。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用情况

使用[Get-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) cmdlet:

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>另请参阅

- [性能的存储空间直接的历史记录](performance-history.md)
