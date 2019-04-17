---
title: 使用 Windows Admin Center 管理虚拟机
description: 管理 HYPER-V 主机和虚拟机使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 374ee30dcbaf9af3caa60ee85ec59fd3c158206b
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296749"
---
# 使用 Windows Admin Center 管理虚拟机

>适用于：Windows Admin Center、Windows Admin Center 预览版

如果服务器或群集上启用了 HYPER-V 角色，该虚拟机工具在[Server](manage-servers.md)、[故障转移群集](manage-failover-clusters.md)或[超融合群集](manage-hyper-converged.md)连接中可用。 你可以使用虚拟机工具来管理 HYPER-V 主机运行 Windows Server 2012 或更高版本，无论是带桌面体验安装或作为安装服务器核心。 HYPER-V Server 2012，2016年和 2019年也受支持。

## 关键功能

Windows Admin Center 中的虚拟机工具的亮点包括：

- **高级 HYPER-V 主机资源监视。** 在单个仪表板中查看总体 CPU 和内存使用情况、 IO 性能指标，虚拟机运行状况警报和事件的 HYPER-V 主机服务器或整个群集。
- **将 HYPER-V 管理器和故障转移群集管理器功能汇集在一起的统一的体验。** 查看跨群集的所有虚拟机并深入到单个虚拟机的高级的管理和疑难解答。
- **虚拟机管理的简化，但功能强大工作流。** 新的 UI 体验定制的 IT 管理方案来创建、 管理和复制虚拟机。

以下是一些你力所能及的 Windows Admin Center 中的 HYPER-V 任务：

- [监视 HYPER-V 主机资源和性能](#monitor-hyper-v-host-resources-and-performance)
- [查看虚拟机库存](#view-virtual-machine-inventory)
- [创建新的虚拟机](#create-a-new-virtual-machine)
- [更改虚拟机设置](#change-virtual-machine-settings)
- [实时迁移到另一个群集节点的虚拟机](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [高级的管理和单个虚拟机的故障排除](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [管理虚拟机通过 HYPER-V 主机 (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [更改 HYPER-V 主机设置](#change-hyper-v-host-settings)
- [查看 HYPER-V 事件日志](#view-hyper-v-event-logs)
- [保护虚拟机使用 Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## 监视 HYPER-V 主机资源和性能

![虚拟机摘要屏幕](../media/manage-virtual-machines/virtual-machines-summary.png)

1. 单击左侧导航窗格中的**虚拟机**工具。
2. 有两个选项卡顶部的**虚拟机**工具、**摘要**选项卡和**清单**选项卡。**摘要**选项卡提供 HYPER-V 主机资源和当前服务器或整个群集，包括以下性能的整体视图：
    - 按状态的运行时，关闭分组的 Vm 数暂停并将其保存
    - 最近的运行状况通知或 HYPER-V 事件日志事件 （通知仅是适用于运行 Windows Server 2016 超融合群集或更高版本）
    - 使用主机与来宾细分的 CPU 和内存使用情况
    - 使用最大 CPU 和内存资源的顶部 Vm
    - 实时和历史记录数据行的 IOPS 和 IO 吞吐量的图表 （存储性能行图表将仅适用于运行 Windows Server 2016 超融合群集或更高版本。 历史记录数据仅适用于运行 Windows Server 2019 的超聚合群集）

## 查看虚拟机库存

![虚拟机库存屏幕](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. 单击左侧导航窗格中的**虚拟机**工具。
2. 有两个选项卡顶部的**虚拟机**工具、**摘要**选项卡和**清单**选项卡。**清单**选项卡列出了适用于当前服务器或整个群集，虚拟机，并提供用于管理单个虚拟机的命令。 您可以：
    - 查看当前服务器或群集上运行的虚拟机的列表。
    - 如果你正在查看的群集虚拟机，请查看虚拟机的状态和主机服务器。 此外可以查看从主机的角度，包括内存压力、 内存需求和分配的内存，和虚拟机的正常运行时间、 检测信号状态以及使用 Azure Site Recovery 保护状态的 CPU 和内存使用情况。
    - [创建新的虚拟机](#create-a-new-virtual-machine)。
    - 删除、 启动、 关闭、 关闭、 暂停、 恢复、 重置或重命名虚拟机。 此外保存虚拟机，删除已保存的状态，或创建一个检查点。
    - [更改虚拟机的设置](#change-virtual-machine-settings)。
    - 连接到虚拟机控制台通过 HYPER-V 主机使用 VMConnect。
    - [复制虚拟机使用 Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)。
    - 对于可在运行多个虚拟机，如开始菜单，关闭，保存，暂停，操作删除，重置，你可以选择多个虚拟机并运行操作在一次。

注意： 如果你已连接到群集，该虚拟机工具将仅显示群集的虚拟机。 我们计划还可以在以后显示非群集虚拟机。

## 创建新的虚拟机

![创建新的虚拟机屏幕](../media/manage-virtual-machines/new-vm.png)

1. 单击左侧导航窗格中的**虚拟机**工具。
2. 在虚拟机工具的顶部，请选择**清单**选项卡，然后单击**新建**创建新的虚拟机。
3. 输入的虚拟机名称和第 1 和 2 代虚拟机之间进行选择。
4. 如果你要在群集上创建虚拟机，你可以选择以开始创建虚拟机上的主机。 如果你运行的 Windows Server 2016 或更高版本，该工具将为你提供主机建议。
5. 选择虚拟机文件的路径。 从下拉列表中选择一个卷，或单击**浏览**选择使用文件夹选取器的文件夹。 虚拟机配置文件和虚拟硬盘文件将保存在单个文件夹下`\Hyper-V\\[virtual machine name]`所选的卷或路径的路径。

>[!Tip]
> 在文件夹选取器，你可以浏览到任何可用 SMB 共享网络上通过在作为**文件夹名称**字段中输入路径```\\server\share```。 使用网络共享进行 VM 存储将需要[CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)。

6. 是否需要启用嵌套虚拟化、 配置内存设置、 网络适配器、 虚拟硬盘和选择是否想要从.iso 映像文件或从网络安装操作系统，请选择虚拟处理器的数量。
7. 单击**创建**以创建虚拟机。
8. 一旦虚拟机将创建并显示在虚拟机列表中，你可以启动虚拟机。
9. 一旦启动虚拟机，你可以连接到虚拟机的控制台通过 VMConnect 安装操作系统。 从列表中选择虚拟机中，单击**更多** > **Connect**下载.rdp 文件。 远程桌面连接应用中打开.rdp 文件。 由于这连接到虚拟机的主机，你将需要输入 HYPER-V 主机管理员凭据。

## 更改虚拟机设置

![虚拟机设置屏幕](../media/manage-virtual-machines/vm-settings.png)

1. 单击左侧导航窗格中的**虚拟机**工具。
2. 在虚拟机工具的顶部，选择**清单**选项卡从列表中选择虚拟机，然后单击**多个** > **设置**。
3. 在**常规**、**安全**、**内存**、**处理器**、**磁盘**、**网络**、**启动顺序**和**检查点**选项卡之间切换，配置必要的设置，然后单击**保存**以保存当前选项卡的设置。 可用的设置有所不同，具体取决于虚拟机的生成。 此外，某些设置不能更改为运行虚拟机，并且将需要首先停止虚拟机。

## 实时迁移到另一个群集节点的虚拟机

如果你已连接到群集，你可以动态将虚拟机迁移到另一个群集节点。

1. 从故障转移群集或超融合群集连接中，单击左侧导航窗格中的**虚拟机**工具。
2. 在虚拟机工具的顶部，选择**清单**选项卡从列表中选择虚拟机，然后单击**多个** > **移动**。
3. 从可用的群集节点的列表中选择服务器，然后单击**移动**。
4. 移动进度通知将显示在 Windows Admin Center 右上角。 如果移动成功，你将看到更改虚拟机列表中的主机服务器名称。

## 高级的管理和单个虚拟机的故障排除

![单个虚拟机的详细信息屏幕](../media/manage-virtual-machines/vm-details.png)

你可以查看详细的信息和从单个虚拟机页面单个虚拟机的性能图表。

1. 单击左侧导航窗格中的**虚拟机**工具。
2. 在虚拟机工具的顶部，从虚拟机列表中选择**清单**选项卡单击虚拟机的名称。
3. 从单个虚拟机页面中，你可以：
    - 查看虚拟机的详细的信息。
    - 查看实时和历史记录数据行图表 CPU、 内存、 网络、 IOPS 和 IO 吞吐量 （仅适用于运行 Windows Server 2019 的超聚合群集历史数据）
    - 查看、 创建、 应用、 重命名和删除检查点。
    - 查看虚拟机的虚拟硬盘 (.vhd) 文件、 网络适配器和主机服务器的详细信息。
    - 删除、 启动、 关闭、 关闭、 暂停、 恢复、 重置或重命名虚拟机。 此外保存虚拟机，删除已保存的状态，或创建一个检查点。
    - [为虚拟机的更改设置](#change-virtual-machine-settings)。
    - 连接到虚拟机控制台通过 HYPER-V 主机使用 VMConnect。
    - [复制虚拟机使用 Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)。

## 管理虚拟机通过 HYPER-V 主机 (VMConnect)

![通过 web 浏览器的虚拟机连接](../media/manage-virtual-machines/vm-connect.png)

1. 单击左侧导航窗格中的**虚拟机**工具。
2. 在虚拟机工具的顶部，选择**清单**选项卡从列表中选择虚拟机，然后单击**多个** > **连接**或**下载 RDP 文件**。 **连接**将允许你与通过远程桌面 web 控制台，在集成到 Windows Admin Center 来宾虚拟机进行交互。 **下载 RDP 文件**将下载可以使用远程桌面连接应用程序 (mstsc.exe) 打开.rdp 文件。 这两个选项将使用 VMConnect 连接到来宾 VM 通过 HYPER-V 主机，并将要求你输入的 HYPER-V 主机服务器管理员凭据。

## 更改 HYPER-V 主机设置

![HYPER-V 主机设置屏幕](../media/manage-virtual-machines/host-settings.png)

1. Server、 超聚合群集或故障转移群集的连接，单击左侧导航窗格底部的**设置**菜单。
2. 在 HYPER-V 主机服务器或群集上，你将看到具有以下各节的**HYPER-V 主机设置**组：
    - 一般： 更改虚拟硬盘和虚拟机的文件路径和虚拟机监控程序计划类型 （如果支持）
    - 增强的会话模式
    - NUMA 跨越
    - 实时迁移
    - 存储迁移
3. 如果你进行任何 HYPER-V 主机中的超聚合群集或故障转移群集连接设置的更改，更改将应用于所有群集节点中。

## 查看 HYPER-V 事件日志

你可以查看 HYPER-V 事件日志直接从虚拟机工具。

1. 单击左侧导航窗格中的**虚拟机**工具。
2. 在虚拟机工具的顶部，选择**摘要**选项卡。在右上角事件部分中，单击**查看所有事件**。
3. 事件查看器工具将显示在左侧窗格中的 HYPER-V 事件通道。 选择要在右侧窗格中查看事件的通道。 如果你正在管理故障转移群集或超融合群集，事件日志将显示所有群集节点，在计算机列中显示主机服务器中的事件。

## 保护虚拟机使用 Azure Site Recovery

你可以使用 Windows Admin Center 来配置 Azure Site Recovery 和本地虚拟机复制到 Azure。 [了解详细信息](../azure/azure-site-recovery.md)

## 多个即将

Windows Admin Center 中的虚拟机管理主动正在开发和不久将添加新功能。 你可以查看状态和功能的投票 UserVoice:

|功能请求|
|-------|
|[导入/导出虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[排序文件夹中的虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[支持其他虚拟机设置](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[HYPER-V 副本支持](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[虚拟机所有权委托](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[克隆虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[从现有的虚拟机创建模板](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[在 HYPER-V 主机上的视图虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[在新的虚拟机窗格中配置 VLAN](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**查看所有或提出新功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
