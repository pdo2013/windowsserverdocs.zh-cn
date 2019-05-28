---
title: 群集关联
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 03/07/2019
description: 本指南介绍了故障转移群集的相关性和 antiAffinity 级别
ms.openlocfilehash: a38d53f6aed1ca634d41822f4486779f6d279ec0
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476054"
---
# <a name="cluster-affinity"></a>群集关联

> 适用于：Windows Server 2019、Windows Server 2016

故障转移群集可以包含大量角色可在节点之间移动和运行。  有些的时候时 （即虚拟机、 资源组等） 的某些角色不应运行在同一节点。  这可能是由于资源消耗、 内存使用情况等。例如，有两个虚拟机的内存和 CPU 资源，并且如果两个虚拟机在同一节点上运行，一个或两个虚拟机可能具有性能影响问题。  本文将介绍群集 antiaffinity 级别以及如何使用它们。

## <a name="what-is-affinity-and-antiaffinity"></a>关联和 AntiAffinity 是什么？

相关性是 （i、 e、 虚拟机、 资源组、 等） 将它们放在一起的两个或多个角色之间建立关系的规则设置。  AntiAffinity 相同，但用于尝试并维持除指定的角色。  故障转移群集使用 AntiAffinity 为其角色。  具体而言， [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames)参数定义的角色，因此它们不在同一节点上运行。  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

在查看组的属性时，没有参数 AntiAffinityClassNames，则为默认值为空白。  在下面的示例中，应从同一节点上运行分隔 Group1 和 Group2。  若要查看的属性，PowerShell 命令，并且结果将：

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

由于 AntiAffinityClassNames 未定义默认情况下，这些角色可以相隔或一起运行。  目标是要将它们分离，从而保留。  AntiAffinityClassNames 的值可以是任何你希望将其，它们只是一定要相同。  假设 Group1 和 Group2 是域控制器虚拟机中运行，并且他们将能够最好地满足不同节点上运行。  由于它们是域控制器，我将使用 DC 的类名。  若要设置的值，PowerShell 命令和结果将：

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

现在，设置，将尝试故障转移群集，以使它们保持分离。  

AntiAffinityClassName 参数是"软"块。  这意味着，它将尝试使它们保持分离，但如果不能它将仍允许它们在同一节点上运行。  例如，组双节点故障转移群集上运行。  如果一个节点需要停机以进行维护，则意味着这两个组将启动并运行在同一节点上。  在这种情况下，它会较这。  不是最理想的但这两个 virtial 计算机将仍在可接受的性能范围内运行。

## <a name="i-need-more"></a>我需要更多

如前文所述，AntiAffinityClassNames 是软块。  但是，如果需要硬块？  虚拟机不能在他上运行同一个节点;否则为对性能的影响将会出现，从而导致某些服务可能下降。

对于这些情况下，没有 ClusterEnforcedAntiAffinity 的其他群集属性。  此 antiaffinity 级别将会阻止在所有成本相同 AntiAffinityClassNames 值中的任何运行在同一节点上。

若要查看的属性和值，PowerShell 命令 （和结果） 将：

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

值为"0"表示它处于禁用状态，而不会强制执行。  "1"的值启用它并且硬块。  若要启用此硬块，命令 （和结果） 是：

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

当这两种设置时，组将无法联机一起。  如果它们是在同一节点上，这是你将看到的内容在故障转移群集管理器。

![群集关联](media\Cluster-Affinity\Cluster-Affinity-1.png)

在组的 PowerShell 列表中，你会看到此：

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>其他注释

- 确保使用的具体取决于需求的适当 AntiAffinity 设置。
- 请记住，在双节点方案中并且 ClusterEnforcedAntiAffinity，如果一个节点处于关闭状态，这两个组将不会运行。  

- 在三个或多个节点群集中，可以使用 AntiAffinity 组合组上的使用首选所有者。
- AntiAffinityClassNames 和 ClusterEnforcedAntiAffinity 设置资源回收后才发生。 I.E. 您可以设置它们，但如果这两个组都处于联机状态在同一节点上设置时，它们都会继续保持联机状态。



