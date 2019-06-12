---
title: 升级故障转移群集使用相同的硬件
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: 本指南介绍了升级使用相同的硬件的 2 节点故障转移群集
ms.localizationpriority: medium
ms.openlocfilehash: 77cde9e64fda385facd91d86483f4d7f749f30a1
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453048"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>在同一硬件上升级故障转移群集

> 适用于：Windows Server 2019、Windows Server 2016

故障转移群集是一组独立的计算机，这些计算机相互协作以提高应用程序和服务的可用性。 多台群集服务器（称为节点）通过物理电缆和软件连接。 如果其中一个群集节点出现故障，另一个节点便会开始提供服务（此过程称为故障转移）。 用户体验服务中断最小的值。

本指南介绍从早期版本使用相同的硬件群集节点升级到 Windows Server 2019 或 Windows Server 2016 的步骤。

## <a name="overview"></a>概述

升级操作系统上将现有故障转移群集时才支持从 Windows Server 2016 转到 Windows 2019。  如果故障转移群集正在运行较早版本，例如如 Windows Server 2012 R2 及更早版本，升级将群集服务运行时将不允许节点联接在一起。  如果使用的相同硬件，可以采取步骤来获取到较新版本。  

在之前故障转移群集的任何升级，请查阅[Windows 升级 Center](https://www.microsoft.com/upgradecenter)。  Windows Server 的就地升级时，你将从现有的操作系统版本到较新的发行版，同时在同一硬件上。 Windows Server 可以升级后的就地至少一个，并有时两个版本前滚。 例如，可以升级 Windows Server 2012 R2 和 Windows Server 2016 位置到 Windows Server 2019。  此外请记住[群集迁移向导](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/)可用，但仅支持最多返回两个版本。 下图显示了适用于 Windows Server 的升级路径。 向下指向箭头表示移动到 Windows Server 2019 的早期版本中支持的升级路径。

![就地升级图示](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

以下步骤是从 Windows Server 2012 故障转移群集服务器转到 Windows Server 2019 使用相同的硬件的示例。  

在开始之前任何升级，请确保在当前备份，包括系统状态。  此外请确保所有驱动程序和固件更新为将使用你的操作系统的认证级别。  未将此处涵盖以下两个说明。

在下面的示例中，故障转移群集的名称是群集和节点名称为 NODE1 和 NODE2。

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>第 1 步：第一个节点中收回并升级到 Windows Server 2016

1. 在故障转移群集管理器中，耗尽所有资源从节点 1 到节点 2 按鼠标右键单击节点并选择**暂停**并**都清空角色**。  或者，可以使用 PowerShell 命令[SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。

    ![清空节点](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. 从通过鼠标右键单击节点，然后选择群集节点 1 中收回**更多操作**并**逐出**。  或者，可以使用 PowerShell 命令[删除节点](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode)。

    ![清空节点](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. 作为预防措施，从正在使用的存储中分离节点 1。  在某些情况下，断开计算机与存储电缆做出就足够了。  检查正确分离步骤根据需要向存储供应商。  具体取决于你的存储，这不可能有必要。

4. 重新生成节点 1 通过 Windows Server 2016。  请确保已添加所有必需的角色、 功能、 驱动程序和安全更新。

5. 创建名为与 NODE1 CLUSTER1 的新群集。  打开故障转移群集管理器并在**管理**窗格中，选择**创建群集**并按照向导中的说明进行操作。

    ![清空节点](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. 创建群集后，将需要从原始的群集迁移到此新的群集角色。  在新群集上右键鼠标单击群集名称 (CLUSTER1)，然后选择**更多操作**并**复制群集角色**。  遵循向导来迁移角色中。

    ![清空节点](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  一旦已迁移的所有资源，关机 NODE2 （原始群集） 和断开连接的存储，以便不会造成任何干扰。  将存储连接到节点 1。  所有连接后，将所有资源联机，并确保它们正常运行也应如此。

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>步骤 2：重新生成为 Windows Server 2019 第二个节点

确认一切按预期工作后，可以重新生成为 Windows Server 2019 NODE2，并将其加入到群集中。

1. 节点 2 上执行 Windows Server 2019 的全新安装。 请确保已添加所有必需的角色、 功能、 驱动程序和安全更新。

2. 现在，已消失的原始群集 （群集），可以保留 CLUSTER1 作为新的群集名称，或返回到原始名称。  如果你想要返回到原始名称，请按照下列步骤：
   
   a. 在节点 1，在故障转移群集管理器中右键单击群集 (CLUSTER1) 的名称，然后选择**属性**。
   
   b. 上**常规**选项卡上，重命名到群集的群集。

   c. 在选择确定或应用时，你将看到下面的对话框弹出窗口。

    ![清空节点](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. 将停止并重新启动才能完成该重命名所需群集服务。

3. 在节点 1 上, 打开故障转移群集管理器。  右键单击**节点**，然后选择**添加节点**。  遍历该向导将 NODE2 添加到群集。

4. 将存储附加到 NODE2。 这可能包括重新连接存储电缆。 

5. 耗尽所有资源从节点 1 到节点 2 按鼠标右键单击节点并选择**暂停**并**都清空角色**。  或者，可以使用 PowerShell 命令[SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。  确保所有资源都都处于联机状态，并且它们的功能也应如此。

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>步骤 3:重新生成为 Windows Server 2019 的第一个节点

1. 从群集中收回节点 1 并断开连接的方式从节点存储在前面。

2. 重新生成或将节点 1 升级到 Windows Server 2019。  请确保已添加所有必需的角色、 功能、 驱动程序和安全更新。

3. 重新附加存储并将节点 1 添加回群集。

4. 将所有资源都移动到节点 1，并确保它们进入联机状态并根据需要函数。

5. 在 Windows 2016 保持当前的群集功能级别。  使用 PowerShell 命令将功能级别更新为 Windows 2019 [UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)。

现在运行的具有完全正常运行的 Windows Server 2019 故障转移群集。

## <a name="additional-notes"></a>其他说明

- 如先前所述，断开连接的存储可能会或可能不需要。  在我们的文档中，我们想要小心。  请咨询存储供应商。
- 如果您的起始点是 Windows Server 2008 或 2008 R2 群集，可能需要通过将其他运行的步骤。
- 如果该群集正在运行的虚拟机，请确保虚拟机级别升级后使用 PowerShell 命令完成了群集功能级别[UPDATE-VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion)。
- 请注意，是否您正在运行 SQL Server、 Exchange Server 等应用程序，该应用程序将不会迁移使用复制群集角色向导。  对于应用程序的正确的迁移步骤，应该咨询你的应用程序供应商。