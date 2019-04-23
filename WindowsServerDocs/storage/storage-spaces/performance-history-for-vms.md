---
title: 为虚拟机的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890868"
---
# <a name="performance-history-for-virtual-machines"></a>为虚拟机的性能历史记录

> 适用于：Windows Server Insider Preview

此子主题[的存储空间直通的性能历史记录](performance-history.md)详细地介绍了为虚拟机 (VM) 收集的性能历史记录。 性能历史记录是可用于每个运行，群集的 VM。

   > [!NOTE]
   > 可能需要几分钟时间集合以开始为新创建的或已重命名的 Vm。

## <a name="series-names-and-units"></a>序列名称和单位

这些系列将收集有关每个符合条件的 VM:

| 系列                            | 单位             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | %          |
| `vm.memory.assigned`              | 字节数            |
| `vm.memory.available`             | 字节数            |
| `vm.memory.maximum`               | 字节数            |
| `vm.memory.minimum`               | 字节数            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | 字节数            |
| `vm.memory.total`                 | 字节数            |
| `vmnetworkadapter.bandwidth.inbound`  | 每秒位数 |
| `vmnetworkadapter.bandwidth.outbound` | 每秒位数 |
| `vmnetworkadapter.bandwidth.total`    | 每秒位数 |

此外，虚拟硬盘 (VHD) 的所有序列，如`vhd.iops.total`，每个 VHD 附加到 VM 的聚合。

## <a name="how-to-interpret"></a>如何解释


| 系列                            | 描述                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | 百分比虚拟机正在使用的主机服务器的处理器。                                   |
| `vm.memory.assigned`              | 分配给虚拟机的内存数量。                                                      |
| `vm.memory.available`             | 保持可用，分配量的内存的数量。                                       |
| `vm.memory.maximum`               | 如果使用动态内存，这是内存的最大数量的可能分配给虚拟机。 |
| `vm.memory.minimum`               | 如果使用动态内存，这是内存的最小数量的可能分配给虚拟机。 |
| `vm.memory.pressure`              | 通过分配给虚拟机的内存所要求的虚拟机的内存的比率。            |
| `vm.memory.startup`               | 要启动的虚拟机所需内存的数量。                                            |
| `vm.memory.total`                 | 总内存。 |
| `vmnetworkadapter.bandwidth.inbound`  | 跨其所有虚拟网络适配器收到的虚拟机的数据的速率。                        |
| `vmnetworkadapter.bandwidth.outbound` | 虚拟机在其所有虚拟网络适配器之间发送的数据的速率。                            |
| `vmnetworkadapter.bandwidth.total`    | 数据的总速率接收或在其所有虚拟网络适配器之间发送的虚拟机。          |

   > [!NOTE]
   > 计数器来度量在整个间隔内，不会被取样。 例如，如果 VM 处于空闲状态 9 秒但在 10 秒，使用 50%的主机 CPU 峰值其`vm.cpu.usage`记录为 5%平均此 10 秒间隔内。 这可确保其性能历史记录捕获所有活动也很可靠到干扰。

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用[GET-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet:

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > GET-VM cmdlet 仅返回虚拟机上的本地 （或指定） 的服务器，不能跨群集。

## <a name="see-also"></a>请参阅

- [有关存储空间直通的性能历史记录](performance-history.md)
