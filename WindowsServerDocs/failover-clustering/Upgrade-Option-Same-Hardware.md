---
title: 升级使用相同的硬件故障转移群集
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: 本文介绍了升级 2 节点故障转移群集中使用相同的硬件
ms.localizationpriority: medium
ms.openlocfilehash: 0bfeb05c8cbc205745dc16bc7ef04052481668ea
ms.sourcegitcommit: 2c2027b597e2483eea8967d0710d65c2247b6751
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2019
ms.locfileid: "9121276"
---
# 在相同的硬件上升级故障转移群集

> 适用于： Windows Server 2019、 Windows Server 2016

故障转移群集是一组独立协同工作以提高应用程序和服务可用性的计算机。 群集服务器（称为节点）通过物理电缆和软件连接。 如果某个群集节点出现故障，另一个节点开始提供服务 （该过程称为故障转移）。 用户体验到最低程度的服务中断。

本指南介绍了从早期版本使用相同的硬件升级到 Windows Server 2019 或 Windows Server 2016 的群集节点的步骤。

## 概述

升级操作系统上现有的故障转移群集时才支持从 Windows Server 2016 转到 Windows 2019。  如果故障转移群集运行早期版本，例如如 Windows Server 2012 R2 及更早版本，升级时正在运行的群集服务将不允许将节点连接起来。  如果使用相同的硬件，则可以采取步骤来获取到较新版本。  

任何在升级之前的故障转移群集，请参阅[Windows 升级中心](https://www.microsoft.com/upgradecenter)。  Windows Server 的就地升级时，你将移动从现有的操作系统版本到较新版本，同时保持在同一硬件上。 Windows Server 可以升级后的就地至少一个，有时向前两个版本。 例如，Windows Server 2012 R2 和 Windows Server 2016 可以升级到 Windows Server 2019 位置中。  此外请记住，[群集迁移向导](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/)可以使用，但仅支持最多返回两个版本。 下图显示了适用于 Windows Server 的升级路径。 指针向下箭头表示移动到 Windows Server 2019 的较早版本的受支持升级路径。

![就地升级的关系图](media\In-Place-Upgrade\In-Place-Upgrade-1.png)

以下步骤是从 Windows Server 2012 故障转移群集服务器转到 Windows Server 2019 使用相同的硬件的示例。  

在开始之前任何升级，请确保已完成当前备份，包括系统状态。  此外需要确保所有驱动程序和固件更新到将使用你的操作系统的认证级别。  这些两个说明将在此处未做介绍。

在下面的示例中，故障转移群集的名称是群集和节点名称是节点 1 和节点 2。

## 步骤 1： 退出第一个节点并升级到 Windows Server 2016

1. 在故障转移群集管理器中，释放所有资源从节点 1 到节点 2 通过鼠标右键单击该节点并选择**暂停**和**都清空角色**。  或者，你可以使用 PowerShell 命令[暂停 CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。

    ![清空节点](media\In-Place-Upgrade\In-Place-Upgrade-2.png)

2. 退出从通过鼠标右键单击节点，然后选择**更多操作**和**逐出**群集节点 1。  或者，你可以使用 PowerShell 命令[REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode)。

    ![清空节点](media\In-Place-Upgrade\In-Place-Upgrade-3.png)

3. 作为一项措施，从你使用的的存储分离节点 1。  在某些情况下，断开与计算机的连接存储电缆就足够了。  请与正确分离步骤如果需要你存储供应商。  存储，可能不是这必要。

4. 重新生成 Windows server 2016 的节点 1。  确保你已添加所有必需的角色、 功能、 驱动程序和安全更新。

5. 创建新的群集调用 CLUSTER1 与节点 1。  打开故障转移群集管理器并在**管理**窗格中，选择**创建群集**，然后按照向导中的说明。

    ![清空节点](media\In-Place-Upgrade\In-Place-Upgrade-4.png)

6. 创建群集后，将需要从原始群集迁移到此新群集角色。  在新群集，鼠标右键单击的群集名称 (CLUSTER1) 和选择**更多操作**和**复制群集角色**。  在向导中迁移角色，请按照。

    ![清空节点](media\In-Place-Upgrade\In-Place-Upgrade-5.png)

7.  一旦已迁移所有资源，关闭节点 2 （原始群集） 和断开连接的存储以不会导致任何干扰。  连接存储到节点 1。  一旦所有已连接，使所有资源联机，并确保它们能达到应。

## 步骤 2： 重新生成到 Windows Server 2019 的第二个节点

验证所有部分都按预期工作后，请可以重建到 Windows Server 2019 节点 2，并将其加入到群集中。

1. 节点 2 上执行干净安装的 Windows Server 2019。 确保你已添加所有必需的角色、 功能、 驱动程序和安全更新。

2. 现在，原始群集 （群集） 已消失，你可以离开 CLUSTER1 将新群集名称或返回到原始名称。  如果你想要返回到原始的名称，请按照下列步骤：
   
   a. 在节点 1，在故障转移群集管理器鼠标右键单击群集 (CLUSTER1) 的名称，然后选择**属性**。
   
   b. 在**常规**选项卡上，将在群集到群集重命名。

   c. 当选择确定或应用，你将看到如下对话框弹出窗口。

    ![清空节点](media\In-Place-Upgrade\In-Place-Upgrade-6.png)

    d. 群集服务将会停止并需要重新启动重命名来完成。

3. 在节点 1，打开故障转移群集管理器。  鼠标右键单击**节点**，然后选择**添加节点**。  完成该向导将节点 2 添加到群集。

4. 将存储附加到节点 2。 这可能包括重新连接存储电缆。 

5. 释放所有资源从节点 1 到节点 2 通过鼠标右键单击该节点并选择**暂停**和**都清空角色**。  或者，你可以使用 PowerShell 命令[暂停 CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。  确保所有资源都处于联机状态，并且它们的功能也应。

## 步骤 3： 重新生成到 Windows Server 2019 的第一个节点

1. 从群集中退出节点 1 和断开与从方式中的节点的连接存储的你之前。

2. 重新生成或升级到 Windows Server 2019 的节点 1。  确保你已添加所有必需的角色、 功能、 驱动程序和安全更新。

3. 重新连接存储，并将节点 1 添加回群集。

4. 将所有资源都移到节点 1，并确保它们进入联机状态，并在必要时起作用。

5. 在 Windows 2016 保持当前的群集功能级别。  使用 PowerShell 命令[更新 CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)Windows 2019 更新的功能级别。

你现在运行完全正常运行 Windows Server 2019 故障转移群集。

## 其他说明

- 如之前所述，断开连接存储，也可能不需要。  在我们的文档，我们想要务必小心。  请咨询存储供应商。
- 如果你的起始点是 Windows Server 2008 或 2008 R2 群集，可能需要通过其他运行的步骤。
- 如果群集正在运行的虚拟机，请确保之后使用 PowerShell 命令[更新 VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion)做了群集功能级别升级虚拟机级别。
- 请注意，是否你正在运行 SQL Server、 Exchange Server 等应用程序，应用程序将不会迁移与复制群集角色向导。  有关应用程序的正确迁移步骤，应查阅你的应用程序供应商。