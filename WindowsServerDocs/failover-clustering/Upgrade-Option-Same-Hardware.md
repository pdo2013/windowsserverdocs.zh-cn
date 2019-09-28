---
title: 使用相同硬件升级故障转移群集
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: 本文介绍如何使用相同的硬件升级2节点故障转移群集
ms.localizationpriority: medium
ms.openlocfilehash: 5fe93f1d43e0c3a1bc4269b585cb9d021d3461aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361396"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>升级相同硬件上的故障转移群集

> 适用于：Windows Server 2019、Windows Server 2016

故障转移群集是一组独立的计算机，这些计算机相互协作以提高应用程序和服务的可用性。 多台群集服务器（称为节点）通过物理电缆和软件连接。 如果其中一个群集节点出现故障，另一个节点便会开始提供服务（此过程称为故障转移）。 用户最少遇到服务中断的情况。

本指南介绍使用相同硬件将群集节点升级到 Windows Server 2019 或 Windows Server 2016 的步骤。

## <a name="overview"></a>概述

仅当从 Windows Server 2016 升级到 Windows 2019 时才支持升级现有故障转移群集上的操作系统。  如果故障转移群集运行的是早期版本（如 Windows Server 2012 R2 和更早版本），则在运行群集服务时升级将不允许将节点加入一起。  如果使用相同硬件，则可以采取步骤将其获取到较新的版本。  

在升级故障转移群集之前，请参阅[Windows Server 升级内容](../upgrade/upgrade-overview.md)。  就地升级 Windows Server 时，你可以从现有的操作系统版本迁移到较新的版本，同时保持同一个硬件。 Windows Server 可以就地升级，也可以至少升级一次。 例如，可以将 Windows Server 2012 R2 和 Windows Server 2016 就地升级到 Windows Server 2019。  另外，请记住，可以使用[群集迁移向导](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/)，但仅支持最多两个版本的备份。 下图显示了 Windows Server 的升级路径。 向下箭头表示从较早的版本到 Windows Server 2019 的支持的升级路径。

![就地升级关系图](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

以下步骤是使用同一个硬件从 Windows Server 2012 故障转移群集服务器到 Windows Server 2019 的示例。  

在开始任何升级之前，请确保已完成当前的备份（包括系统状态）。  还要确保已将所有驱动程序和固件更新为要使用的操作系统的认证级别。  这里不会介绍这两个注意事项。

在下面的示例中，故障转移群集的名称是群集，节点名称是节点1和节点2。

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>第 1 步：逐出第一个节点并升级到 Windows Server 2016

1. 在故障转移群集管理器中，通过右键单击节点并选择**暂停**和**排出角色**，将所有资源从节点2排出到节点2。  或者，可以使用 PowerShell 命令[start-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。

    ![排出节点](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. 通过右键单击节点并选择 "**更多操作**" 并**退出**，从群集中逐出节点1。  或者，可以使用 PowerShell 命令[start-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode)。

    ![排出节点](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. 作为预防措施，请从正在使用的存储分离节点1。  在某些情况下，断开计算机上的存储电缆的连接将足够。  如有必要，请与存储供应商联系以获取适当的分离步骤。  这可能不是必要的，具体取决于你的存储。

4. 重新生成具有 Windows Server 2016 的节点1。  确保已添加所有必要的角色、功能、驱动程序和安全更新。

5. 使用节点1创建名为 CLUSTER1 的新群集。  打开故障转移群集管理器并在 "**管理**" 窗格中，选择 "**创建群集**"，然后按照向导中的说明进行操作。

    ![排出节点](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. 创建群集后，需要将角色从原始群集迁移到此新群集。  在新群集上，右键单击群集名称（CLUSTER1），并选择 "**更多操作**" 和 "**复制群集角色**"。  按照向导中的步骤迁移角色。

    ![排出节点](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  迁移完所有资源后，关闭节点2（原始群集）并断开存储，以免造成干扰。  将存储连接到节点1。  所有资源都处于连接状态后，请使所有资源联机并确保它们正常运行。

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>步骤 2：将第二个节点重新生成到 Windows Server 2019

验证所有内容是否正常工作后，可将节点2重建为 Windows Server 2019 并加入群集。

1. 在节点2上执行 Windows Server 2019 的干净安装。 确保已添加所有必要的角色、功能、驱动程序和安全更新。

2. 现在，原始群集（群集）已消失，可以保留新群集名称为 "CLUSTER1" 或返回到原始名称。  如果希望返回到原始名称，请执行以下步骤：
   
   a. 在节点1上故障转移群集管理器鼠标右键单击群集的名称（CLUSTER1），然后选择 "**属性**"。
   
   b. 在 "**常规**" 选项卡上，将群集重命名为 cluster。

   c. 选择 "确定" 或 "应用" 时，会看到以下对话框弹出窗口。

    ![排出节点](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. 群集服务将停止，并需要重新启动才能完成重命名。

3. 在节点1上，打开故障转移群集管理器。  右键单击**节点**，然后选择 "**添加节点**"。  完成向导将节点2添加到群集。

4. 将存储附加到节点2。 这可能包括重新连接存储电缆。 

5. 通过右键单击节点并选择**暂停**和**排出角色**，将所有资源从节点2排出到节点2。  或者，可以使用 PowerShell 命令[start-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。  确保所有资源均处于联机状态，并且它们按预期方式工作。

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>步骤 3:重新生成 Windows Server 2019 的第一个节点

1. 从群集中逐出节点1，并以您之前的方式断开存储与节点的连接。

2. 将节点1重建或升级到 Windows Server 2019。  确保已添加所有必要的角色、功能、驱动程序和安全更新。

3. 重新附加存储并将节点1添加回群集。

4. 将所有资源移到节点1，确保它们处于联机状态并在必要时工作。

5. 当前群集功能级别仍在 Windows 2016 上。  通过 PowerShell 命令[update-clusterfunctionallevel](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)将功能级别更新为 Windows 2019。

现在，你正在运行的 Windows Server 2019 故障转移群集功能完全正常。

## <a name="additional-notes"></a>附加说明

- 如前文所述，断开存储可能是必需的，也可能不是必需的。  在本文档中，我们希望注意到错误。  请咨询你的存储供应商。
- 如果您的起点是 Windows Server 2008 或 2008 R2 群集，则可能需要执行额外的步骤。
- 如果群集正在运行虚拟机，请确保在完成群集功能级别后，通过 PowerShell 命令[VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion)升级虚拟机级别。
- 请注意，如果正在运行 SQL Server、Exchange Server 等应用程序，则不会使用复制群集角色向导来迁移应用程序。  应该咨询应用程序供应商，以获取应用程序的适当迁移步骤。