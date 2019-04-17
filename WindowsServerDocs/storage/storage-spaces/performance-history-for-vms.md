---
title: 为虚拟机的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239214"
---
# 为虚拟机的性能历史记录

> 适用于： Windows Server Insider Preview

为虚拟机 (VM) 的性能历史记录收集的详细介绍了[为存储空间直通的性能历史记录](performance-history.md)此子主题。 性能历史记录仅适用于每个运行时，群集的 VM。

   > [!NOTE]
   > 它可能需要几分钟时间开始新创建的或重命名的 Vm 的集合。

## 系列名称和单位

这些系列将收集有关每个符合条件的虚拟机：

| 系列                            | 单位             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | %          |
| `vm.memory.assigned`              | 字节            |
| `vm.memory.available`             | 字节            |
| `vm.memory.maximum`               | 字节            |
| `vm.memory.minimum`               | 字节            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | 字节            |
| `vm.memory.total`                 | 字节            |
| `vmnetworkadapter.bandwidth.inbound`  | 每秒的速度的位 |
| `vmnetworkadapter.bandwidth.outbound` | 每秒的速度的位 |
| `vmnetworkadapter.bandwidth.total`    | 每秒的速度的位 |

此外，所有虚拟硬盘 (VHD) 系列，如`vhd.iops.total`，对于每个 VHD 连接到虚拟机进行聚合。

## 如何解释


| 系列                            | 描述                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | 百分比虚拟机其主机服务器的处理器的使用。                                   |
| `vm.memory.assigned`              | 内存分配到虚拟机的数量。                                                      |
| `vm.memory.available`             | 始终可用，金额分配的内存的数量。                                       |
| `vm.memory.maximum`               | 如果使用动态内存，这是内存的可以分配给虚拟机的最大数量。 |
| `vm.memory.minimum`               | 如果使用动态内存，这是内存的可以分配给虚拟机的最小数量。 |
| `vm.memory.pressure`              | 通过内存分配给虚拟机的虚拟机所需的内存的比率。            |
| `vm.memory.startup`               | 为虚拟机启动所需的内存数量。                                            |
| `vm.memory.total`                 | 总内存。 |
| `vmnetworkadapter.bandwidth.inbound`  | 所有其虚拟网络适配器的虚拟机收到的数据的速率。                        |
| `vmnetworkadapter.bandwidth.outbound` | 跨所有其虚拟网络适配器的虚拟机发送的数据的速率。                            |
| `vmnetworkadapter.bandwidth.total`    | 数据的总速率接收或跨所有其虚拟网络适配器发送的虚拟机。          |

   > [!NOTE]
   > 计数器测量通过不采样的整个间隔。 例如，如果 VM 为 9 秒，但峰值截至第二个，用于 50%的主机 CPU 空闲其`vm.cpu.usage`将记录为 5%平均此 10 秒间隔。 这将确保其性能历史捕获所有活动并就可以防止噪音。

## 在 PowerShell 中的使用情况

使用[虚拟机 Get](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet:

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > 获取虚拟机 cmdlet 只返回虚拟机在本地 （或指定） 服务器上，不是跨群集。

## 另请参阅

- [存储空间直通的性能历史记录](performance-history.md)
