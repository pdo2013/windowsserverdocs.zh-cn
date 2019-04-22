---
title: 了解并查看存储重新同步
description: 当存储重新同步发生以及如何在 Windows Server 2019 中看到它的详细的信息。
keywords: 存储空间直通、 存储重新同步重新同步存储，S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813458"
---
# <a name="understand-and-monitor-storage-resync"></a>了解并监视存储重新同步

>适用于：Windows Server 2019

存储重新同步警报是一新功能[存储空间直通](storage-spaces-direct-overview.md)中允许运行状况服务以引发错误时重新同步你的存储的 Windows Server 2019。 警报可在通知你时发生重新同步，以便不会意外地采取更多的服务器列表 （这会导致多个容错域会受到影响，从而导致群集停止运行）。 

本主题提供了背景和步骤，以了解并查看存储在 Windows Server 故障转移群集使用存储空间直通中的重新同步。

## <a name="understanding-resync"></a>了解重新同步

让我们开始一个简单的示例，若要了解如何存储进行同步。请记住的任何无共享 （本地驱动器仅） 分布式的存储解决方案演示这一行为。 将如下所示，如果一个服务器节点出现故障，则不会更新其驱动器，直到它重新联机的这适用于任何超聚合体系结构。 

假设我们想要存储字符串"HELLO"。 

![字符串"你好"的 ASCII](media/understand-storage-resync/hello.png)

我们有三向镜像复原则 Asssuming，我们有此字符串的三个副本。 现在，如果我们暂时 （对于蝴） 使服务器 #1，则我们无法访问复制 #1。

![不能访问副本 #1](media/understand-storage-resync/copy1.png)

假设我们更新我们从"HELLO"的字符串为"HELP ！" 在这一次。

![字符串"help ！"的 ASCII](media/understand-storage-resync/help.png)

一旦我们更新字符串，#2 和快照 #3 的副本将位于已成功更新。 但是，复制 #1 仍无法访问，因为服务器 #1 已关闭暂时 （适用于蝴）。 

![编写复制 #2 和快照 #2 的 Gif"](media/understand-storage-resync/write.gif)

现在，我们必须具有不同步的数据的复制 #1。操作系统使用精细的脏区域跟踪来跟踪不同步的位。这样，当服务器 #1 重新联机时，我们可以通过从 #2 或 3 的副本中读取数据并覆盖复制 #1 中的数据同步所做的更改。 此方法的优点是，我们只需复制对已过时，而不是重新同步所有服务器 #2 或服务器 #3 中的数据的数据。

![Gif 图像被覆盖以复制 #1"](media/understand-storage-resync/overwrite.gif)

因此，本部分说明如何获取同步的数据。但是，此外观是什么样在高级别？ 假定此示例中，我们有三个服务器超聚合群集。 维护服务器 #1 时，你会将其视为已关闭。 当服务器 #1 将重新启动时，它将启动重新同步使用精细脏区域跟踪 （如上所述） 及其存储的所有。 所有服务器重新同步数据后，将都显示为已开启。

![重新同步的管理视图的 Gif"](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>如何监视存储在 Windows Server 2019 的重新同步

现在，你了解存储重新同步的工作原理，让我们看如何这体现在 Windows Server 2019。 我们已添加到新的错误[运行状况服务](../../failover-clustering/health-service-overview.md)的时，会显示你的存储正在重新同步。

若要在 PowerShell 中查看此错误，请运行：

``` PowerShell
Get-HealthFault
```

这是在 Windows Server 2019 中新的错误，将显示在 PowerShell 中，在群集验证报告中，和任何其他位置，基于运行状况错误。 

若要获取更深入的视图，您可以按如下所示查询时间序列数据库在 PowerShell 中：

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

值得注意的是，Windows Admin Center 使用运行状况错误设置的状态和群集节点的颜色。 因此，此新的错误将导致群集节点，以从红色 （深） 转换，为黄色 （正在重新同步） 为绿色 （向上），而不直接从红色转变成绿色，HCI 仪表板上。

![2016 vs 2019 视图的重新同步的映像"](media/understand-storage-resync/compare.png)

通过显示总体存储重新同步进度，您可以准确地知道多少数据的同步和您的系统是否使进程向前推进。 当打开 Windows Admin Center 并转到*仪表板*，您将看到新警报，如下所示：

![在 Windows Admin Center 中的警报的映像"](media/understand-storage-resync/alert.png)

警报可在通知你时发生重新同步，以便不会意外地采取更多的服务器列表 （这会导致多个容错域会受到影响，从而导致群集停止运行）。 

如果导航到*服务器*Windows Admin Center 在页上，单击*清单*，然后选择特定的服务器，就可以在每台服务器上的此存储重新同步外观的更详细的视图。 如果导航到你的服务器并查看*存储*图中，您将看到需要修复中的数据量*紫色*的精确数目的行的正上方。 此数量将增加时服务器已关闭 （更多数据需要进行重新同步），并在服务器重新联机时，逐渐降低 （要同步的数据）。 在需要修复为 0 的数据量，你的存储是完成重新同步-现将免费可以根据需要向下使服务器。 这种体验 Windows Admin Center 中的屏幕截图如下所示：

![Windows Admin Center 中的服务器视图的映像"](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>如何查看存储在 Windows Server 2016 中的重新同步

正如您所看到的此警报是在获取在存储层发生了什么情况的整体视图特别有用。 有效地总结了可以获取来自 Get-storagejob cmdlet 返回有关长时间运行的存储模块作业，例如存储空间上的修复操作的信息的信息。 示例如下所示：

```PowerShell
Get-StorageJob
```

下面是示例输出：

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

此视图是更精细，因为列出的存储作业是每个卷，可以看到正在运行的作业的列表并且你可以跟踪其各个进度。 此 cmdlet 适用于 Windows Server 2016 和 2019年。

## <a name="see-also"></a>请参阅

- [使服务器脱机以进行维护](maintain-servers.md)
- [存储空间直通概述](storage-spaces-direct-overview.md)