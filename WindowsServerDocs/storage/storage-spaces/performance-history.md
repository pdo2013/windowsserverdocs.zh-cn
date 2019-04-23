---
title: 有关存储空间直通的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870858"
---
# <a name="performance-history-for-storage-spaces-direct"></a>有关存储空间直通的性能历史记录

> 适用于：Windows Server 2019

性能历史记录是一项新功能，使[存储空间直通](storage-spaces-direct-overview.md)管理员轻松访问历史跨主机服务器、 驱动器、 卷、 虚拟机和的详细信息的计算、 内存、 网络和存储度量值。 性能历史记录是自动收集、 存储在群集上一年。

   > [!IMPORTANT]
   > 此功能是 Windows Server 2019 中的新增功能。 不在 Windows Server 2016 中可用。

## <a name="get-started"></a>立即开始行动

默认情况下，使用存储空间直通在 Windows Server 2019 中收集性能历史记录。 不需要安装、 配置或启动任何内容。 不需要 Internet 连接、 System Center 不是必需的和不需要外部数据库。

若要以图形方式查看群集的性能历史记录，请使用[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![在 Windows Admin Center 中的性能历史记录](media/performance-history/perf-history-in-wac.png)

若要查询并以编程方式对其进行处理，使用新`Get-ClusterPerf`cmdlet。 请参阅[在 PowerShell 中的使用情况](#usage-in-powershell)。

## <a name="whats-collected"></a>收集哪些信息

为 7 种类型的对象收集性能历史记录：

![类型的对象](media/performance-history/types-of-object.png)

每个对象类型具有多个系列： 例如，`ClusterNode.Cpu.Usage`将收集的每个服务器。

有关所收集的每个对象类型以及如何解释它们的详细信息，请访问以下子主题：

| Object             | 系列                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| 驱动器             | [所收集的驱动器](performance-history-for-drives.md)                     |
| 网络适配器   | [所收集的网络适配器](performance-history-for-network-adapters.md) |
| 服务器            | [对于服务器收集哪些信息](performance-history-for-servers.md)                   |
| 虚拟硬盘 | [所收集的虚拟硬盘](performance-history-for-vhds.md)           |
| 虚拟机   | [所收集的虚拟机](performance-history-for-vms.md)              |
| 卷            | [对于卷收集哪些信息](performance-history-for-volumes.md)                   |
| 群集           | [对于群集收集哪些信息](performance-history-for-clusters.md)                 |

很多系列合计到其父级的对等对象： 例如，`NetAdapter.Bandwidth.Inbound`单独为每个网络适配器收集和聚合到整体服务器; 同样`ClusterNode.Cpu.Usage`聚合到整个群集;，依此类推。

## <a name="timeframes"></a>时间范围

性能历史记录将存储最多一年，以逐渐减小的粒度。 最近一个小时，度量值可每隔十秒。 此后，它们将以智能方式合并 （通过求平均值或求和，视具体情况） 到更粗粒度系列跨更多的时间。 对于最新的日期，度量值是可用每隔五分钟;最新一周，每 15 分钟;等等。

在 Windows Admin Center，可以选择在图表上方右上方的时间范围。

![在 Windows Admin Center 中的时间范围](media/performance-history/timeframes-in-honolulu.png)

在 PowerShell 中，使用`-TimeFrame`参数。

以下是可用的时间范围：

| 时间范围   | 测量频率 | 保留 |
|-------------|-----------------------|--------------|
| `LastHour`  | 每隔 10 秒         | 1 小时       |
| `LastDay`   | 每隔 5 分钟       | 25 个小时     |
| `LastWeek`  | 每 15 分钟      | 8 天       |
| `LastMonth` | 每隔 1 小时          | 35 天      |
| `LastYear`  | 1 天           | 400 天     |

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用`Get-ClusterPerformanceHistory`cmdlet 在 PowerShell 中的查询和处理性能历史记录。

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > 使用**Get ClusterPerf**别名以保存一些击键。

### <a name="example"></a>示例

获取虚拟机的 CPU 使用情况*MyVM* ，最后一个小时的：

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

有关更高级示例，请参阅已发布[示例脚本](performance-history-scripting.md)提供起始代码要查找最大值、 计算平均值、 绘制趋势线，请运行离群值检测和的详细信息。

### <a name="specify-the-object"></a>指定的对象

可以指定由管道所需的对象。 这适用于 7 种类型的对象：

| 从管道对象 | 示例     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

如果未指定，则返回整个群集的性能历史记录。

### <a name="specify-the-series"></a>指定序列

可以使用这些参数来指定所需的序列：


| 参数                 | 示例                       | 列表                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [所收集的驱动器](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [所收集的网络适配器](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [对于服务器收集哪些信息](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [所收集的虚拟硬盘](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [所收集的虚拟机](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [对于卷收集哪些信息](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [对于群集收集哪些信息](performance-history-for-clusters.md)                 |


   > [!TIP]
   > 使用 tab 自动补全来发现可用的序列。

如果未指定，则返回每个系列可用的指定对象。

### <a name="specify-the-timeframe"></a>指定的时间范围

可以指定您想要的历史记录的时间范围`-TimeFrame`参数。

   > [!TIP]
   > 使用 tab 自动补全来发现可用的时间范围。

如果未指定`MostRecent`返回度量值。

## <a name="how-it-works"></a>工作原理

### <a name="performance-history-storage"></a>性能历史记录存储

稍后启用存储空间直通后，约为 10 GB 的卷名为`ClusterPerformanceHistory`创建且存在配置的可扩展存储引擎 (也称为 Microsoft JET) 实例。 此轻量数据库存储而无需任何管理员参与或管理的性能历史记录。

![性能历史记录存储的卷](media/performance-history/perf-history-volume.png)

该卷由存储空间提供支持，并使用简单的双向镜像或三向镜像复原则，具体取决于群集中的节点数量。 修复后的驱动器或服务器故障就像任何其他卷中存储空间直通。

卷使用 ReFS，但不是群集共享卷 (CSV)，因此，它仅显示在群集组所有者节点上。 除了自动创建，并没有什么特别与该卷： 可以查看、 浏览它、 调整其大小，或删除它 （不推荐）。 如果出现问题，请参阅[故障排除](#troubleshooting)。 

### <a name="object-discovery-and-data-collection"></a>对象发现和数据收集

性能历史记录自动发现相关的对象，例如虚拟机，在群集中的任意位置，并开始传输及其性能计数器。 计数器是聚合、 同步，并且插入到数据库。 流式处理连续运行和优化最少的系统产生影响。

集合由运行状况服务，它是高度可用： 如果运行位置所在的节点出现故障，它将恢复时间更高版本对另一群集中的节点。 性能历史记录会花费一些简单地说，但它将自动恢复。 可以通过运行来查看运行状况服务，其所有者节点`Get-ClusterResource Health`在 PowerShell 中。

### <a name="handling-measurement-gaps"></a>处理度量缺口

当度量值将合并到更粗粒度系列跨更多的时间，如中所述的[时段](#Timeframes)，排除缺少数据的时间段。 例如，如果服务器已关闭的 30 分钟内，然后运行 50%的 CPU 在接下来的 30 分钟内，`ClusterNode.Cpu.Usage`的平均时间为一小时将正确记录为 50%（而不是 25%)。

### <a name="extensibility-and-customization"></a>可扩展性和自定义项

性能历史记录是适合脚本编写的。 使用 PowerShell 提取任何可用的历史记录，直接从数据库将自动报告或警报，导出历史记录以保证安全，请滚动你自己的可视化效果，等等。请参阅已发布[示例脚本](performance-history-scripting.md)的有用的起始代码。

不能收集其他对象、 时间范围或系列的历史记录。

测量频率和保留期不是当前可配置的。

## <a name="start-or-stop-performance-history"></a>启动或停止性能历史记录

### <a name="how-do-i-enable-this-feature"></a>如何启用此功能？

除非您`Stop-ClusterPerformanceHistory`，默认情况下启用性能历史记录。

若要重新启用它，请以管理员身份运行此 PowerShell cmdlet:

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>如何禁用此功能？

若要停止收集性能历史记录，请以管理员身份运行此 PowerShell cmdlet:

```PowerShell
Stop-ClusterPerformanceHistory
```

若要删除现有度量值，请使用`-DeleteHistory`标志：

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > 在初始部署期间您可以防止性能历史记录启动通过设置`-CollectPerformanceHistory`的参数`Enable-ClusterStorageSpacesDirect`到`$False`。

## <a name="troubleshooting"></a>疑难解答

### <a name="the-cmdlet-doesnt-work"></a>该 cmdlet 不起作用

一条错误消息等"*术语 Get ClusterPerf 无法识别为 cmdlet 名称*"意味着该功能不可用或已安装。 验证你拥有 17692 或更高版本的 Windows Server Insider Preview 版本，你已安装故障转移群集，以及可以运行存储空间直通。

   > [!NOTE]
   > 此功能不是适用于 Windows Server 2016 或更早版本。

### <a name="no-data-available"></a>没有可用的数据 

如果图表显示了"*没有可用的数据*"如图所示，下面介绍了如何进行故障排除：

![没有可用的数据](media/performance-history/no-data-available.png)

1. 如果新添加或创建该对象，等待它为发现 （最多 15 分钟）。

2. 刷新页面，或者等待下一步的后台刷新 （最多 30 秒）。

3. 某些特殊的对象不在性能历史记录-例如，不群集的虚拟机和不使用群集共享卷 (CSV) 文件系统的卷。 检查子主题的对象类型，如[卷的性能历史记录](performance-history-for-volumes.md)的要点。

4. 如果问题仍然存在，请打开 PowerShell，以管理员身份运行`Get-ClusterPerf`cmdlet。 该 cmdlet 包括疑难解答逻辑以识别常见的问题，例如，如果 ClusterPerformanceHistory 卷缺少，并提供补救说明。

5. 如果在上一步中的命令不返回任何内容，则可以尝试通过运行重新启动运行状况服务 （它收集性能历史记录）`Stop-ClusterResource Health ; Start-ClusterResource Health`在 PowerShell 中。

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
