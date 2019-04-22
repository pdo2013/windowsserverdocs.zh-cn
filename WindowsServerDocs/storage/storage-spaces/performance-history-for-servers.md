---
title: 服务器的的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820588"
---
# <a name="performance-history-for-servers"></a>服务器的的性能历史记录

> 适用于：Windows Server Insider Preview

此子主题[的存储空间直通的性能历史记录](performance-history.md)详细地介绍了为服务器收集的性能历史记录。 性能历史记录是可用于在群集中的每个服务器。

   > [!NOTE]
   > 不能为已关闭的服务器收集性能历史记录。 当服务器重新启动后，集合将自动恢复。

## <a name="series-names-and-units"></a>序列名称和单位

这些系列是针对每个合格的服务器收集的：

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

此外，如驱动器系列`physicaldisk.size.total`聚合了所有符合条件的驱动器附加到服务器和网络适配器系列如`networkadapter.bytes.total`聚合了所有符合条件的网络适配器连接到服务器。

## <a name="how-to-interpret"></a>如何解释

| 系列                           | 如何解释                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | 不处于空闲状态的处理器时间的百分比。                        |
| `clusternode.cpu.usage.guest`    | 使用来宾 （虚拟机） 需求的处理器时间的百分比。 |
| `clusternode.cpu.usage.host`     | 主机需为使用的处理器时间的百分比。                    |
| `clusternode.memory.total`       | 服务器的物理内存总量。                              |
| `clusternode.memory.available`   | 服务器的可用内存。                                   |
| `clusternode.memory.usage`       | 服务器分配的 （不可用） 内存。                   |
| `clusternode.memory.usage.guest` | 分配给来宾 （虚拟机） 需求的内存。               |
| `clusternode.memory.usage.host`  | 分配给主机需的内存。                                  |

## <a name="where-they-come-from"></a>这些来自

`cpu.*`系列从具体取决于是否启用 HYPER-V 的不同性能计数器收集。

如果启用了 HYPER-V:

| 系列                           | 源计数器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

使用`% Total Run Time`计数器可确保性能历史记录属性所有使用情况。

如果未启用 HYPER-V:

| 系列                           | 源计数器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *总体使用情况相同* |

不完善的同步，尽管`clusternode.cpu.usage`始终`clusternode.cpu.usage.host`加上`clusternode.cpu.usage.guest`。

使用的同一注意事项`clusternode.cpu.usage.guest`始终是总和`vm.cpu.usage`的主机服务器上的所有虚拟机。

`memory.*`系列是 （即将推出）。

  > [!NOTE]
  > 计数器来度量在整个间隔内，不会被取样。 例如，如果服务器的空闲时间 9 秒到 100%的 CPU 峰值在 10 秒，其`clusternode.cpu.usage`记录为 10%平均此 10 秒间隔内。 这可确保其性能历史记录捕获所有活动也很可靠到干扰。

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用[Get-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) cmdlet:

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>请参阅

- [有关存储空间直通的性能历史记录](performance-history.md)
