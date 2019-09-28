---
title: 了解并查看存储重新同步
description: 有关存储重新同步发生时间以及如何在 Windows Server 2019 中查看它的详细信息。
keywords: 存储空间直通，存储重新同步，重新同步，存储，S2D
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 53f48421bddd416d24c5f46e53652cc89c10c785
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402847"
---
# <a name="understand-and-monitor-storage-resync"></a>了解并监视存储重新同步

>适用于：Windows Server 2019

存储重新同步警报是 Windows Server 2019 中[存储空间直通](storage-spaces-direct-overview.md)的一项新功能，允许运行状况服务在重新同步存储时引发错误。 警报可用于在重新同步发生时通知你，因此你不会意外地将更多服务器关闭（这可能会导致多个容错域受到影响，导致群集停止）。 

本主题提供了有关使用存储空间直通了解 Windows Server 故障转移群集中的存储重新同步的背景和步骤。

## <a name="understanding-resync"></a>了解重新同步

让我们从一个简单的示例开始，了解存储如何同步。请记住，任何共享的（仅限本地驱动器）分布式存储解决方案都有这种行为。 正如你将看到的，如果一个服务器节点出现故障，则其驱动器将不会更新，直到它恢复联机状态-任何超聚合体系结构都是如此。 

假设我们要存储字符串 "HELLO"。 

![字符串 "hello" 的 ASCII](media/understand-storage-resync/hello.png)

Asssuming 有三向镜像复原，我们有三个此字符串的副本。 现在，如果我们暂时将服务器 #1 下来（对于维护），则无法访问复制 #1。

![无法访问复制 #1](media/understand-storage-resync/copy1.png)

假设我们将字符串从 "HELLO" 更新为 "HELP！" 此时。

![字符串 "help！" 的 ASCII](media/understand-storage-resync/help.png)

更新字符串后，将成功更新 "复制 #2 和 #3。 但是，仍无法访问复制 #1，因为服务器 #1 暂时关闭（对于维护）。 

![用于复制 #2 和 #2 的 Gif](media/understand-storage-resync/write.gif)

现在，复制 #1 包含不同步的数据。操作系统使用粒度脏区域跟踪来跟踪不同步的位。这样，当服务器 #1 恢复联机状态时，可以通过从复制 #2 或 #3 并覆盖 copy #1 中的数据来同步更改。 此方法的优点是，我们只需复制陈旧的数据，而不是重新同步服务器 #2 或服务器 #3 中的所有数据。

![要复制 #1 的 Gif](media/understand-storage-resync/overwrite.gif)

因此，这说明了数据不同步的方式。但这在很大程度上看起来如何呢？ 假设在此示例中，我们有三个服务器超聚合群集。 服务器 #1 处于维护状态时，你会看到它已关闭。 将服务器 #1 备份时，它将使用精细的脏区域跟踪（如上所述）开始重新同步其所有存储。 数据全部返回同步后，所有服务器都将显示为 "up"。

![重新同步的管理视图的 Gif](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>如何监视 Windows Server 2019 中的存储重新同步

现在，你已了解存储重新同步的工作原理，让我们看看它在 Windows Server 2019 中的显示方式。 我们向[运行状况服务](../../failover-clustering/health-service-overview.md)中添加了一个新的错误，在重新同步存储时将显示此错误。

若要在 PowerShell 中查看此错误，请运行：

``` PowerShell
Get-HealthFault
```

这是 Windows Server 2019 中的新错误，将在 PowerShell 中、在群集验证报告中显示，并显示在运行状况错误的其他任何位置。 

若要获取更深入的视图，可以在 PowerShell 中查询时序数据库，如下所示：

```PowerShell
Get-ClusterNode | Get-ClusterPerf -ClusterNodeSeriesName ClusterNode.Storage.Degraded
```
下面是一些示例输出：

```
Object Description: ClusterNode Server1

Series                       Time                Value Unit
------                       ----                ----- ----
ClusterNode.Storage.Degraded 01/11/2019 16:26:48     214 GB
```

特别要注意的是，Windows 管理中心使用运行状况错误来设置群集节点的状态和颜色。 因此，这种新的故障会导致在 HCI 仪表板上将群集节点从红色（下）转换为黄色（重新同步），而不是从红色转换为绿色。

![2016与2019的重新同步视图的图像](media/understand-storage-resync/compare.png)

通过显示总体存储重新同步进度，你可以准确了解有多少数据不同步，以及你的系统是否正在前进进度。 当你打开 Windows 管理中心并中转到*仪表板*时，你将看到如下所示的新警报：

![Windows 管理中心的警报图像](media/understand-storage-resync/alert.png)

警报可用于在重新同步发生时通知你，因此你不会意外地将更多服务器关闭（这可能会导致多个容错域受到影响，导致群集停止）。 

如果导航到 Windows 管理中心中的 "*服务器*" 页，请单击 "*清单*"，然后选择特定服务器，你可以更详细地了解如何在每个服务器基础上查看此存储重新同步的方式。 如果你导航到你的服务器并查看*存储*图表，你将看到需要在前面带有精确数字的*紫色*行中修复的数据量。 当服务器停机（需要 resynced 更多的数据）时，此数量会增加，并且在服务器恢复联机状态（正在同步数据）时逐渐降低。 当需要修复的数据量为0时，存储已完成重新同步，如果需要，你现在可以随时关闭服务器。 Windows 管理中心中的此体验的屏幕截图如下所示：

![Windows 管理中心中服务器视图的图像](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>如何查看 Windows Server 2016 中的存储重新同步

正如您所看到的，此警报在获取存储层发生的情况的整体视图时特别有用。 它有效地汇总了你可以从 Get-storagejob cmdlet 获取的信息，该 cmdlet 返回有关长时间运行的存储模块作业的信息，例如存储空间上的修复操作。 下面显示了一个示例：

```PowerShell
Get-StorageJob
```

下面是示例输出：

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

此视图的粒度更细，因为列出的存储作业是每个卷的，你可以查看正在运行的作业的列表，你可以跟踪其各个进度。 此 cmdlet 适用于 Windows Server 2016 和2019。

## <a name="see-also"></a>请参阅

- [使服务器脱机以进行维护](maintain-servers.md)
- [存储空间直通概述](storage-spaces-direct-overview.md)