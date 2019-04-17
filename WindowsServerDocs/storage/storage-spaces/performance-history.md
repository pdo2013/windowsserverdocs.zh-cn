---
title: 存储空间直通的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239254"
---
# 存储空间直通的性能历史记录

> 适用于： Windows Server 2019

性能历史记录是一项新功能，使[存储空间直通](storage-spaces-direct-overview.md)管理员能够轻松访问历史计算、 内存、 网络和存储测量跨主机服务器、 驱动器、 卷、 虚拟机和详细信息。 自动收集和存储群集上最多一年性能历史记录。

   > [!IMPORTANT]
   > 此功能是 Windows Server 2019 中的新增功能。 不可用在 Windows Server 2016。

## 入门

默认情况下，存储空间直通在 Windows Server 2019 收集性能历史记录。 你不需要安装、 配置或开始菜单的任何内容。 并不需要 Internet 连接、 系统中心不是必需的和外部数据库不是必需。

若要查看的群集的性能历史记录以图形方式，使用[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Windows Admin Center 中的性能历史记录](media/performance-history/perf-history-in-wac.png)

若要查询并以编程方式对其进行处理，使用新`Get-ClusterPerf`cmdlet。 请参阅[在 PowerShell 中的使用情况](#usage-in-powershell)。

## 什么被收集

为 7 类型的对象收集性能历史记录：

![类型的对象](media/performance-history/types-of-object.png)

每个对象类型都具有许多系列： 例如，`ClusterNode.Cpu.Usage`是为每个服务器收集。

有关每个对象类型收集的内容以及如何解释它们的详细信息，请参阅这些子主题：

| 对象             | 系列                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| 驱动器             | [什么被收集驱动器](performance-history-for-drives.md)                     |
| 网络适配器   | [什么被收集的网络适配器](performance-history-for-network-adapters.md) |
| 服务器            | [对于服务器收集](performance-history-for-servers.md)                   |
| 虚拟硬盘 | [什么是为虚拟硬盘收集](performance-history-for-vhds.md)           |
| 虚拟机   | [什么被收集的虚拟机](performance-history-for-vms.md)              |
| 卷            | [什么被收集的卷](performance-history-for-volumes.md)                   |
| 群集           | [对于群集收集](performance-history-for-clusters.md)                 |

许多系列聚合跨对等对象到其父级： 例如，`NetAdapter.Bandwidth.Inbound`是单独为每个网络适配器收集和聚合到服务器的总体;同样`ClusterNode.Cpu.Usage`聚合到整个群集中;依次类推。

## 时间范围

具有不断缩短粒度达一年，存储性能历史记录。 对于最新小时，测量值是可用每隔 10 秒。 此后，将智能地合并这些 （通过平均或汇总，根据） 到不太精确系列跨越更多的时间。 最近一天，测量值是可用每五分钟;有关最新的每周，每隔 15 分钟;依次类推。

在 Windows Admin Center，你可以选择在右上方在图表之上的时间。

![Windows Admin Center 中的时间范围](media/performance-history/timeframes-in-honolulu.png)

在 PowerShell 中，使用`-TimeFrame`参数。

下面是可用的时间范围：

| 时间范围   | 度量频率 | 为保留 |
|-------------|-----------------------|--------------|
| `LastHour`  | 每隔 10 秒         | 1 小时       |
| `LastDay`   | 每 5 分钟       | 25 个小时     |
| `LastWeek`  | 每隔 15 分钟      | 8 天       |
| `LastMonth` | 每 1 小时          | 35 天      |
| `LastYear`  | 每个 1 天           | 400 天     |

## 在 PowerShell 中的使用情况

使用`Get-ClusterPerformanceHistory`cmdlet 可以执行以下 PowerShell 中的查询和过程性能历史记录。

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > **获取 ClusterPerf**别名用于保存一些击键。

### 示例

获取过去一小时的虚拟机*MyVM*的 CPU 使用率：

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

有关更多高级示例，请参阅提供起始代码来查找峰值值、 计算平均值、 绘制趋势线、 检测和详细信息，请运行 outlier 发布的[示例脚本](performance-history-scripting.md)。

### 指定的对象

你可以指定你希望通过管道的对象。 这适用于 7 类型的对象：

| 从管道对象 | 示例     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

如果你未指定，将返回的总体群集的性能历史记录。

### 指定一系列

你可以使用这些参数指定所需的系列：


| 参数                 | 示例                       | 列表                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [什么被收集驱动器](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [什么被收集的网络适配器](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [对于服务器收集](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [什么是为虚拟硬盘收集](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [什么被收集的虚拟机](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [什么被收集的卷](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [对于群集收集](performance-history-for-clusters.md)                 |


   > [!TIP]
   > 使用 tab 键完成发现可用系列。

如果你未指定，则返回可用于指定每个系列。

### 指定时间范围

你可以指定要使用的历史记录的时间范围`-TimeFrame`参数。

   > [!TIP]
   > 使用 tab 键完成发现可用的时间范围。

如果你未指定，`MostRecent`返回测量。

## 工作原理

### 性能历史记录存储

大约 10 GB 的卷不久启用存储空间直通后，名为`ClusterPerformanceHistory`创建和那里预配可扩展存储引擎 (也称为 Microsoft JET) 的实例。 此轻量级数据库存储而无需任何管理员参与或管理的性能历史记录。

![性能历史记录存储的卷](media/performance-history/perf-history-volume.png)

该卷受存储空间，并使用简单的双向镜像或三向镜像复原，具体取决于群集中节点的数量。 它会修复后驱动器或服务器故障就像存储空间直通中的任何其他卷。

卷使用 ReFS，但不是群集共享卷 (CSV)，使其仅出现在群集组所有者节点上。 除了自动创建，没有任何特殊关于此卷： 你可以看到它、 浏览、 调整其大小，或删除它 （不推荐）。 如果出现问题，请参阅[疑难解答](#troubleshooting)。 

### 对象发现和数据收集

性能历史记录自动发现相关的对象，例如虚拟机，群集中的任意位置，并开始流式其性能计数器。 计数器聚合、 同步，以及插入到数据库。 流式处理连续运行和针对最少的系统影响进行了优化。

集合处理运行状况服务，这是高度可用： 当运行所在的位置的节点关闭时，它将恢复时刻稍后在另一群集中的节点。 性能历史记录中会花费一些简单，但它将自动恢复。 你可以通过运行看到运行状况服务和其所有者节点`Get-ClusterResource Health`在 PowerShell 中。

### 处理度量间隙

当测量合并到不太精确跨越更多时间，[时间范围](#Timeframes)中所述的系列时，排除期间丢失的数据。 例如，如果服务器 30 分钟内关闭，然后运行在 50 %cpu 接下来的 30 分钟，`ClusterNode.Cpu.Usage`平均为小时正确将记录为 50%（而不是 25%)。

### 可扩展性和自定义

性能历史记录是脚本友好。 使用 PowerShell 来拉取直接从数据库以生成自动化报告或警报导出妥善保管的历史记录任何可用历史记录、 倾斜你自己的可视化效果，等等。请参阅很有帮助的起始代码发布[示例脚本](performance-history-scripting.md)。

不能收集的其他对象、 时间范围或系列历史记录。

不可当前配置的测量频率和保留期。

## 开始或停止性能历史记录

### 如何启用此功能？

除非你`Stop-ClusterPerformanceHistory`，性能历史记录在默认情况下处于启用状态。

若要重新启用它，请以管理员身份运行此 PowerShell cmdlet:

```PowerShell
Start-ClusterPerformanceHistory
```

### 如何禁用此功能？

若要停止收集性能历史记录，请以管理员身份运行此 PowerShell cmdlet:

```PowerShell
Stop-ClusterPerformanceHistory
```

若要删除现有的度量值，请使用`-DeleteHistory`标志：

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > 初始在部署期间，你可以阻止性能历史记录启动通过设置`-CollectPerformanceHistory`参数`Enable-ClusterStorageSpacesDirect`到`$False`。

## 疑难解答

### 该 cmdlet 不起作用

一条错误消息，如"*术语 Get-ClusterPerf 无法识别的 cmdlet 名称为*"是指功能不可用或已安装。 验证你有 17692 或更高版本的 Windows Server Insider Preview 版本，你已安装故障转移群集，并且你正在运行存储空间直通。

   > [!NOTE]
   > 此功能不适用于 Windows Server 2016 或更早版本。

### 没有可用的数据 

如果图表显示"*没有可用的数据*"，如图中显示，下面介绍了如何进行疑难解答：

![没有可用的数据](media/performance-history/no-data-available.png)

1. 如果新添加或创建该对象，请等待其可发现 （最多 15 分钟）。

2. 刷新页面，则等待下一次后台刷新 （最多 30 秒为单位）。

3. 从性能历史记录-例如，不组成群集的虚拟机，并不使用群集共享卷 (CSV) 文件系统的卷中排除某些特殊的对象。 检查的对象类型，如[卷的性能历史记录](performance-history-for-volumes.md)，精细打印的子主题。

4. 如果问题仍然存在，打开 PowerShell 作为管理员，然后运行`Get-ClusterPerf`cmdlet。 该 cmdlet 包含一些疑难解答逻辑识别常见的问题，例如如果 ClusterPerformanceHistory 卷丢失，并提供修正说明。

5. 如果在上一步中的命令返回任何内容，则可以尝试重启运行状况服务 （用于收集性能历史记录），通过运行`Stop-ClusterResource Health ; Start-ClusterResource Health`在 PowerShell 中。

## 另请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
