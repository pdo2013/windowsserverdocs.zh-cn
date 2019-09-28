---
title: 群集关联
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 03/07/2019
description: 本文介绍故障转移群集相关性和 antiAffinity 级别
ms.openlocfilehash: 9a269d2b14e953daee849008a473c750dfbfe84b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361464"
---
# <a name="cluster-affinity"></a>群集关联

> 适用于：Windows Server 2019、Windows Server 2016

故障转移群集可以拥有多个可在节点间移动和运行的角色。  有时某些角色（例如虚拟机、资源组等）不应在同一个节点上运行。  这可能是由于资源消耗、内存使用情况等引起的。例如，有两个虚拟机占用大量内存和 CPU，如果这两个虚拟机运行在同一节点上，则一个或两个虚拟机可能会影响性能。  本文将介绍群集 antiaffinity 级别以及如何使用它们。

## <a name="what-is-affinity-and-antiaffinity"></a>什么是相关性和 AntiAffinity？

相关性是您设置的一种规则，用于在两个或多个角色（i、e、虚拟机、资源组等）之间建立关系，以便将它们保持在一起。  AntiAffinity 是相同的，但用于尝试将指定的角色彼此分开。  故障转移群集使用 AntiAffinity 作为其角色。  更具体地说，是对角色定义的[AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames)参数，使其不会在同一节点上运行。  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

查看组的属性时，有参数 AntiAffinityClassNames，默认值为空。  在下面的示例中，应将 "Group1" 和 "Group2" 分隔为在同一节点上运行。  若要查看属性，PowerShell 命令和结果将为：

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

由于 AntiAffinityClassNames 未定义为默认值，因此这些角色可以一起运行或分离。  目标是将它们分开。  AntiAffinityClassNames 的值可以是您想要的任何值，而只是必须是相同的值。  假设，Group1 和 Group2 是在虚拟机中运行的域控制器，它们在不同的节点上运行是最佳的。  由于这些是域控制器，因此，我将使用 DC 作为类名。  若要设置值，PowerShell 命令和结果将为：

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

现在已设置了这些设置，故障转移群集将尝试将它们分开。  

AntiAffinityClassName 参数是一个 "软" 块。  也就是说，它会尝试将其保持不变，但如果不能，它仍将允许它们在同一节点上运行。  例如，组在两节点故障转移群集上运行。  如果某个节点需要关闭以进行维护，则这意味着两个组将在同一节点上启动并运行。  在这种情况下，可以这样做。  它可能不是最理想的，但这两个 virtial 计算机在可接受的性能范围内仍将运行。

## <a name="i-need-more"></a>我需要更多

如前所述，AntiAffinityClassNames 是一个软块。  但如果需要一个硬块会怎样呢？  虚拟机不能在该节点上运行;否则，会发生性能影响，并导致某些服务可能会中断。

对于这些情况，有一个 ClusterEnforcedAntiAffinity 的附加群集属性。  此 antiaffinity 级别将阻止在同一节点上运行任何相同的 AntiAffinityClassNames 值。

若要查看属性和值，PowerShell 命令（和结果）应为：

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

如果值为 "0"，则表示它已禁用且不会被强制执行。  如果值为 "1"，则它是一个硬块。  若要启用此硬块，命令（和结果）为：

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

如果同时设置了这两个设置，则会阻止组联机。  如果它们在同一个节点上，这就是在故障转移群集管理器中看到的内容。

![群集关联](media/Cluster-Affinity/Cluster-Affinity-1.png)

在组的 PowerShell 列表中，会看到以下内容：

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>其他注释

- 请确保使用正确的 AntiAffinity 设置，具体取决于需要。
- 请记住，在双节点方案和 ClusterEnforcedAntiAffinity 中，如果一个节点关闭，则这两个组将无法运行。  

- 对组使用首选所有者可与三个或更多节点群集中的 AntiAffinity 结合使用。
- AntiAffinityClassNames 和 ClusterEnforcedAntiAffinity 设置仅在资源回收后发生。 I.E. 可以设置这些组，但如果在设置时两个组在同一节点上处于联机状态，则它们都将继续保持联机状态。



