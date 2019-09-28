---
title: 用 Windows 管理中心管理虚拟机
description: 通过 Windows 管理中心管理 Hyper-v 主机和虚拟机（Project Honolulu）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 02a33383b466e8bade2db0bbddaff66f0196954c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356859"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>用 Windows 管理中心管理虚拟机

>适用于：Windows Admin Center、Windows Admin Center 预览版

如果在服务器或群集上启用了 Hyper-v 角色，则可以在[服务器](manage-servers.md)、[故障转移群集](manage-failover-clusters.md)或[超聚合群集](manage-hyper-converged.md)连接中使用虚拟机工具。 你可以使用虚拟机工具来管理运行 Windows Server 2012 或更高版本的 Hyper-v 主机，该主机可以使用桌面体验或服务器核心安装。 还支持 hyper-v Server 2012、2016和2019。

## <a name="key-features"></a>关键功能

Windows 管理中心中的 "虚拟机" 工具的亮点包括：

- **高级别 Hyper-v 主机资源监视。** 在单个仪表板中查看 Hyper-v 主机服务器或整个群集的总体 CPU 和内存使用情况、IO 性能指标、VM 运行状况警报和事件。
- **使 Hyper-v 管理器和故障转移群集管理器功能结合起来的统一体验。** 查看整个群集中的所有虚拟机，并向下钻取到单个虚拟机以进行高级管理和故障排除。
- **为虚拟机管理简化但功能强大的工作流。** 为 IT 管理方案定制的新 UI 体验可用于创建、管理和复制虚拟机。

下面是可以在 Windows 管理中心执行的一些 Hyper-v 任务：

- [监视 Hyper-v 主机资源和性能](#monitor-hyper-v-host-resources-and-performance)
- [查看虚拟机清单](#view-virtual-machine-inventory)
- [创建新的虚拟机](#create-a-new-virtual-machine)
- [更改虚拟机设置](#change-virtual-machine-settings)
- [将虚拟机实时迁移到另一个群集节点](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [针对单个虚拟机的高级管理和故障排除](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [通过 Hyper-v 主机（VMConnect）管理虚拟机](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [更改 Hyper-v 主机设置](#change-hyper-v-host-settings)
- [查看 Hyper-v 事件日志](#view-hyper-v-event-logs)
- [通过 Azure Site Recovery 保护虚拟机](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>监视 Hyper-v 主机资源和性能

![虚拟机摘要屏幕](../media/manage-virtual-machines/virtual-machines-summary.png)

1. 单击左侧导航窗格中的 "**虚拟机**" 工具。
2. **虚拟机**工具顶部有两个选项卡： "**摘要**" 选项卡和 "**清单**" 选项卡。"**摘要**" 选项卡为当前服务器或整个群集提供 hyper-v 主机资源和性能的整体视图，包括以下各项：
    - 按状态（正在运行、已关闭、已暂停和已保存）分组的 Vm 数
    - 最近的运行状况警报或 Hyper-v 事件日志事件（警报仅适用于运行 Windows Server 2016 或更高版本的超聚合群集）
    - 主机与来宾细目的 CPU 和内存使用情况
    - 占用最多 CPU 和内存资源的顶级 Vm
    - 用于 IOPS 和 IO 吞吐量的实时和历史数据折线图（存储性能折线图仅适用于运行 Windows Server 2016 或更高版本的超聚合群集。 历史数据仅适用于运行 Windows Server 2019 的超聚合群集

## <a name="view-virtual-machine-inventory"></a>查看虚拟机清单

![虚拟机库存屏幕](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. 单击左侧导航窗格中的 "**虚拟机**" 工具。
2. **虚拟机**工具顶部有两个选项卡： "**摘要**" 选项卡和 "**清单**" 选项卡。"**清单**" 选项卡列出了当前服务器或整个群集上可用的虚拟机，并提供用于管理单个虚拟机的命令。 你可以：
    - 查看在当前服务器或群集上运行的虚拟机的列表。
    - 查看虚拟机的状态和主机服务器（如果正在查看群集的虚拟机）。 还可查看主机透视中的 CPU 和内存使用情况，包括内存压力、内存需求和分配的内存，以及使用 Azure Site Recovery 的虚拟机的运行时间、检测信号状态和保护状态。
    - [创建新的虚拟机](#create-a-new-virtual-machine)。
    - 删除、启动、关闭、关闭、暂停、继续、重置或重命名虚拟机。 同时保存虚拟机、删除已保存状态或创建检查点。
    - [更改虚拟机的设置](#change-virtual-machine-settings)。
    - 通过 Hyper-v 主机使用 VMConnect 连接到虚拟机控制台。
    - [使用 Azure Site Recovery 复制虚拟机](#protect-virtual-machines-with-azure-site-recovery)。
    - 对于可在多个 Vm 上运行的操作，例如 "启动"、"关闭"、"保存"、"暂停"、"删除" 和 "重置"，可以选择多个 Vm 并同时运行操作。

注意：如果已连接到群集，则虚拟机工具将仅显示群集虚拟机。 我们计划还会在将来显示非群集虚拟机。

## <a name="create-a-new-virtual-machine"></a>创建新的虚拟机

!["新建虚拟机" 屏幕](../media/manage-virtual-machines/new-vm.png)

1. 单击左侧导航窗格中的 "**虚拟机**" 工具。
2. 在 "虚拟机" 工具的顶部，选择 "**清单**" 选项卡，然后单击 "**新建**" 以创建新的虚拟机。
3. 输入虚拟机名称，并在第1代和第2代虚拟机之间进行选择。
4. 如果要在群集上创建虚拟机，可以选择最初在其中创建虚拟机的主机。 如果你运行的是 Windows Server 2016 或更高版本，则该工具将为你提供主机建议。
5. 选择虚拟机文件的路径。 从下拉列表中选择一个卷，或者单击 "**浏览**" 以使用文件夹选取器选择一个文件夹。 虚拟机配置文件和虚拟硬盘文件将保存在选定卷或路径的`\Hyper-V\\[virtual machine name]`路径下的单个文件夹中。

   >[!Tip]
   > 在文件夹选取器中，可以通过在 "**文件夹名称**" 字段中输入路径 ```\\server\share```，浏览到网络上任何可用的 SMB 共享。 为 VM 存储使用网络共享需要[CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)。

6. 选择虚拟处理器数量，无论是要启用嵌套虚拟化、配置内存设置、网络适配器、虚拟硬盘，还是选择要从 .iso 映像文件或网络安装操作系统。
7. 单击 **“创建”** 创建虚拟机。
8. 创建虚拟机并将其显示在虚拟机列表中后，可以启动虚拟机。
9. 启动虚拟机后，可以通过 VMConnect 连接到虚拟机的控制台以安装操作系统。 从列表中选择虚拟机，单击 "**更多** > **连接**" 下载 .rdp 文件。 在远程桌面连接应用中打开 .rdp 文件。 由于这将连接到虚拟机的控制台，因此需要输入 Hyper-v 主机的管理员凭据。

## <a name="change-virtual-machine-settings"></a>更改虚拟机设置

![虚拟机设置屏幕](../media/manage-virtual-machines/vm-settings.png)

1. 单击左侧导航窗格中的 "**虚拟机**" 工具。
2. 在 "虚拟机" 工具的顶部，选择 "**清单**" 选项卡。从列表中选择一个虚拟机，然后单击 "**更多** > "**设置**。
3. 在 "**常规**"、"**安全**"、"**内存**"、"**处理器**"、"**磁盘**"、"**网络**"、"**启动顺序**" 和 "**检查点**" 选项卡当前选项卡的设置。 可用的设置取决于虚拟机的生成。 此外，某些设置无法更改以运行虚拟机，你将需要先停止虚拟机。

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>将虚拟机实时迁移到另一个群集节点

如果已连接到群集，则可以将虚拟机实时迁移到另一个群集节点。

1. 从故障转移群集或超聚合群集连接中，单击左侧导航窗格中的 "**虚拟机**" 工具。
2. 在 "虚拟机" 工具的顶部，选择 "**清单**" 选项卡。从列表中选择一个虚拟机，然后单击 "**更多** > **移动**"。
3. 从可用群集节点列表中选择服务器，然后单击 "**移动**"。
4. 移动进度的通知将显示在 Windows 管理中心的右上角。 如果移动成功，则会在虚拟机列表中看到主机服务器名称已更改。

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>针对单个虚拟机的高级管理和故障排除

![单个虚拟机详细信息屏幕](../media/manage-virtual-machines/vm-details.png)

你可以从 "单一虚拟机" 页查看单个虚拟机的详细信息和性能图表。

1. 单击左侧导航窗格中的 "**虚拟机**" 工具。
2. 在 "虚拟机" 工具的顶部，选择 "**清单**" 选项卡。从 "虚拟机" 列表中单击虚拟机的名称。
3. 在单个虚拟机页上，你可以：
    - 查看虚拟机的详细信息。
    - 查看 CPU、内存、网络、IOPS 和 IO 吞吐量的实时和历史数据折线图（历史数据仅适用于运行 Windows Server 2019 的超聚合群集）
    - 查看、创建、应用、重命名和删除检查点。
    - 查看虚拟机的虚拟硬盘（.vhd）文件、网络适配器和主机服务器的详细信息。
    - 删除、启动、关闭、关闭、暂停、继续、重置或重命名虚拟机。 同时保存虚拟机、删除已保存状态或创建检查点。
    - [更改虚拟机的设置](#change-virtual-machine-settings)。
    - 通过 Hyper-v 主机使用 VMConnect 连接到虚拟机控制台。
    - [使用 Azure Site Recovery 复制虚拟机](#protect-virtual-machines-with-azure-site-recovery)。

## <a name="manage-a-virtual-machine-through-the-hyper-v-host-vmconnect"></a>通过 Hyper-v 主机（VMConnect）管理虚拟机

![通过 web 浏览器连接 VM](../media/manage-virtual-machines/vm-connect.png)

1. 单击左侧导航窗格中的 "**虚拟机**" 工具。
2. 在 "虚拟机" 工具的顶部，选择 "**清单**" 选项卡。从列表中选择一个虚拟机，然后单击 "**更多** > **连接**" 或 "**下载 RDP 文件**"。 **连接**将允许你通过集成到 Windows 管理中心的远程桌面 web 控制台与来宾 VM 交互。 **下载 rdp 文件**将下载可使用远程桌面连接应用程序（mstsc）打开的 .rdp 文件。 这两个选项都将使用 VMConnect 通过 Hyper-v 主机连接到来宾 VM，并将要求你输入 Hyper-v 主机服务器的管理员凭据。

## <a name="change-hyper-v-host-settings"></a>更改 Hyper-v 主机设置

![Hyper-v 主机设置屏幕](../media/manage-virtual-machines/host-settings.png)

1. 在服务器、超聚合群集或故障转移群集连接上，单击左侧导航窗格底部的 "**设置**" 菜单。
2. 在 Hyper-v 主机服务器或群集上，你将看到一个**Hyper-v 主机设置**组，其中包含以下部分：
    - 常规更改虚拟硬盘和虚拟机文件路径，以及虚拟机监控程序计划类型（如果支持）
    - 增强会话模式
    - NUMA 跨越
    - 实时迁移
    - 存储迁移
3. 如果在超聚合群集或故障转移群集连接中进行任何 Hyper-v 主机设置更改，则更改将应用到所有群集节点。

## <a name="view-hyper-v-event-logs"></a>查看 Hyper-v 事件日志

你可以直接从虚拟机工具查看 Hyper-v 事件日志。

1. 单击左侧导航窗格中的 "**虚拟机**" 工具。
2. 在 "虚拟机" 工具的顶部，选择 "**摘要**" 选项卡。在右上方的事件部分中，单击 "**查看所有事件**"。
3. 事件查看器工具将在左窗格中显示 Hyper-v 事件通道。 选择通道，在右窗格中查看事件。 如果要管理故障转移群集或超聚合群集，事件日志将显示所有群集节点的事件，并在计算机列中显示主机服务器。

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>通过 Azure Site Recovery 保护虚拟机

你可以使用 Windows 管理中心来配置 Azure Site Recovery 并将本地虚拟机复制到 Azure。 [了解详细信息](../azure/azure-site-recovery.md)

## <a name="more-coming"></a>更多

Windows 管理中心中的虚拟机管理正在开发中主动，不久后将添加新功能。 可以查看 UserVoice 中的功能的状态和投票：

- [导入/导出虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)
- [对文件夹中的虚拟机进行排序](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)
- [支持其他虚拟机设置](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)
- [Hyper-v 副本支持](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)
- [委派虚拟机所有权](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)
- [克隆虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)
- [从现有虚拟机创建模板](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)
- [跨 Hyper-v 主机查看虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)
- [在新建虚拟机窗格中配置 VLAN](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)

[查看全部或建议新功能](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)。
