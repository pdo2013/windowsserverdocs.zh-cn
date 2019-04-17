---
title: 了解并查看存储重新同步
description: 当存储重新同步发生时，以及如何在 Windows Server 2019 中看到它的详细的信息。
keywords: 存储空间直通，存储重新同步，重新同步，存储，S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 748eccd10fc0c3c962d6e64ff6ead08017ac1947
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2019
ms.locfileid: "9009594"
---
# 了解并监视存储重新同步

>适用于： Windows Server 2019

存储重新同步警报是[存储空间直通](storage-spaces-direct-overview.md)允许引发故障时重新同步存储运行状况服务的 Windows Server 2019 中的新功能。 警报非常有用时通知您发生重新同步，以便不会意外地执行更多服务器向下 （这会导致多个容错域要受到影响，从而向下转群集中）。 

本主题提供了背景和了解并查看存储重新同步具有存储空间直通的 Windows Server 故障转移群集中的步骤。

## 了解重新同步

我们来看一个简单的示例，以了解如何存储获取同步。请记住，任何共享 （本地驱动器仅） 分布式的存储解决方案展示此行为。 你将看到下面，如果一个服务器节点出现故障，则不会更新其驱动器，直到变回联机-这是适用于适用于任何超聚合体系结构。 

假设我们想要存储字符串"HELLO"。 

![字符串"你好"的 ASCII](media/understand-storage-resync/hello.png)

我们有三向镜像复原 Asssuming，我们有此字符串的三个副本。 现在，我们会关闭临时 （对于 maintanence) 服务器 1，如果我们无法访问副本 #1。

![无法访问副本 #1](media/understand-storage-resync/copy1.png)

假设我们更新我们从"HELLO"的字符串为"帮助 ！" 在此时间。

![ASCII 字符串"帮助 ！"](media/understand-storage-resync/help.png)

一旦我们更新的字符串，则会成功更新副本 #2 和 3。 但是，复制 #1 仍无法访问，因为服务器 #1 已关闭临时 （适用于 maintanence)。 

![编写复制 #2 和 #2 的 Gif"](media/understand-storage-resync/write.gif)

现在，我们必须复制 #1 已同步的数据。操作系统将使用跟踪以跟踪不同步的位的细化已更新区域。这样，当服务器 1 变回联机时，我们可以通过从复制 #2 或 3 读取数据并覆盖复制 #1 中的数据同步所做的更改。 此方法的优点是，我们只需复制已过时，而不是正在重新同步服务器 2 或服务器 #3 中的数据的所有数据。

![覆盖的 Gif 复制 #1"](media/understand-storage-resync/overwrite.gif)

因此，这可以说明如何获取同步的数据。但看起来是怎样在高级别上？ 假定本示例中，我们有三个服务器的超聚合群集。 维护服务器 1 时，你将看到它视为关闭。 当重新向上打开服务器 1 时，它将开始重新同步所有使用 （上述） 细化脏乱区域跟踪其存储。 所有重新同步数据后，将显示所有服务器，最多。

![管理员视图的重新同步的 Gif"](media/understand-storage-resync/admin.gif)

## 如何监视 Windows Server 2019 中的存储重新同步

现在，你了解存储重新同步的工作原理，让我们看如何显示在 Windows Server 2019 中。 我们已添加到你存储正在重新同步时将显示[运行状况服务](../../failover-clustering/health-service-overview.md)的新容错。

若要查看此故障在 PowerShell 中运行：

``` PowerShell
Get-HealthFault
```

这是 Windows Server 2019 中的新故障，并且将显示在 PowerShell 中，在群集验证报告中，并且其他任何位置的版本上运行状况故障。 

若要获取更深入地视图，你可以如下所示查询时系列数据库在 PowerShell 中：

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

值得注意的是，Windows Admin Center 使用运行状况故障设置的状态和颜色的群集节点。 因此，此新的故障将导致从红色过渡 （向下） 为黄色 （正在重新同步） 为绿色 （向上），而不直接从红色转到绿色的群集节点 HCI 仪表板上。

![2016 vs 2019 视图的重新同步的映像"](media/understand-storage-resync/compare.png)

通过显示的整体存储重新同步进度，可以准确地知道多少数据是同步的并且你的系统是否进行向前进度。 打开 Windows Admin Center 并转到*仪表板*，你将看到新的警报，如下所示：

![Windows Admin Center 中的警报的图像"](media/understand-storage-resync/alert.png)

警报非常有用时通知您发生重新同步，以便不会意外地执行更多服务器向下 （这会导致多个容错域要受到影响，从而向下转群集中）。 

如果你导航到 Windows Admin Center 中的*服务器*页面，单击*库存*，，然后选择特定的服务器，你可以获取此存储重新同步在每台服务器上的外观的详细的视图。 如果你导航到你的服务器，并查看*存储*图表，你将看到需要修复的确切数量*紫色*行中的数据量上方。 此时间将增加时服务器已关闭 （更多数据需要进行重新同步），并逐渐减少在服务器重新联机时 （正在同步数据）。 当需要修复为 0 的数据，存储量完成正在重新同步-你可以立即自由地会关闭服务器，如果你需要。 Windows Admin Center 中的此体验的屏幕截图所示：

![Windows Admin Center 中的服务器视图的图像"](media/understand-storage-resync/server.png)

## 如何查看 Windows Server 2016 中的存储重新同步

如你所见，此警报将获取存储层发生的情况的整体视图尤其有用。 有效地总结了你可以从获取 StorageJob cmdlet，它将返回有关长时间运行存储模块作业，如修复操作上的存储空间信息获取的信息。 示例如下所示：

```PowerShell
Get-StorageJob
```

下面是示例输出：

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

此视图是大量更精细的因为列出存储作业是每个卷，你可以看到正在运行的作业的列表，你可以跟踪其个人的进度。 此 cmdlet 在 Windows Server 2016 和 2019年上工作。

## 另请参阅

- [使服务器脱机以进行维护](maintain-servers.md)
- [存储空间直通概述](storage-spaces-direct-overview.md)