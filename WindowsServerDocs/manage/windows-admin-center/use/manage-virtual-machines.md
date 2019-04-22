---
title: 管理虚拟机与 Windows Admin Center
description: 管理 HYPER-V 主机和虚拟机与 Windows Admin Center (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 41767b9e53c0106931e78f86f8675e413cca0d0a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816878"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>管理虚拟机与 Windows Admin Center

>适用于：Windows Admin Center，Windows Admin Center 预览版

虚拟机工具现已推出[服务器](manage-servers.md)，[故障转移群集](manage-failover-clusters.md)或[Hyper-Converged 群集](manage-hyper-converged.md)连接如果服务器或群集上启用 HYPER-V 角色。 可以使用虚拟机工具来管理 HYPER-V 主机运行 Windows Server 2012 或更高版本，或者安装具有桌面体验或作为 Server Core。 此外支持 Hyper-V Server 2012 和 2016年。

## <a name="key-features"></a>关键功能

Windows Admin Center 中的虚拟机工具的亮点包括：

- **高级的 HYPER-V 主机资源监视。** 在单个仪表板中查看总体 CPU 和内存使用情况、 IO 性能指标、 VM 运行状况警报和事件的 HYPER-V 主机服务器或整个群集。
- **组合在一起的 HYPER-V 管理器和故障转移群集管理器功能的统一的体验。** 查看跨群集的所有虚拟机和向下钻取到单个虚拟机进行高级的管理和故障排除。
- **虚拟机管理的简化，而又强大工作流。** 新的 UI 体验定制 IT 管理方案来创建、 管理和复制虚拟机。

下面是一些可以执行 Windows Admin Center 中的 HYPER-V 任务：

- [监视 HYPER-V 主机资源和性能](#monitor-hyper-v-host-resources-and-performance)
- [查看虚拟机清单](#view-virtual-machine-inventory)
- [创建新的虚拟机](#create-a-new-virtual-machine)
- [更改虚拟机设置](#change-virtual-machine-settings)
- [实时迁移到另一个群集节点的虚拟机](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [高级的管理和故障排除的单个虚拟机](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [查看 HYPER-V 事件日志](#view-hyper-v-event-logs)
- [使用 Azure Site Recovery 保护虚拟机](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>监视 HYPER-V 主机资源和性能

![虚拟机摘要屏幕](../media/manage-virtual-machines/virtual-machines-summary.png)

1. 单击**虚拟机**从左侧导航窗格中的工具。
2. 有两个选项卡的顶部**虚拟机**工具，**摘要**选项卡并**清单**选项卡。**摘要**选项卡提供了全面的 HYPER-V 主机资源和性能有关当前服务器或整个群集，其中包括：
    - 按状态-运行，off，分组的 Vm 的数目已暂停，并保存
    - 最近的运行状况警报或 HYPER-V 事件日志事件 （警报是仅可用于超聚合群集运行 Windows Server 2016 或更高版本）
    - 与主机与来宾细分的 CPU 和内存使用情况
    - 使用最多 CPU 和内存资源的顶部 Vm
    - 实时和历史数据行的 IOPS 和 IO 吞吐量图表 （存储性能折线图是仅可用于超聚合群集运行 Windows Server 2016 或更高版本。 历史数据功能仅适用于运行 Windows Server 2019 的超聚合群集）

## <a name="view-virtual-machine-inventory"></a>查看虚拟机清单

![虚拟机清单屏幕](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. 单击**虚拟机**从左侧导航窗格中的工具。
2. 有两个选项卡的顶部**虚拟机**工具，**摘要**选项卡并**清单**选项卡。**清单**选项卡列出可在当前服务器或整个群集上的虚拟机，并提供用于管理单个虚拟机的命令。 你可以：
    - 查看当前服务器或群集上运行的虚拟机的列表。
    - 如果您正在查看群集的虚拟机，请查看虚拟机的状态和主机服务器。 此外查看从主机的角度，包括内存不足的情况、 内存需求和已分配的内存和虚拟机的运行时间、 检测信号状态以及使用 Azure Site Recovery 的保护状态的 CPU 和内存使用情况。
    - [创建新的虚拟机](#create-a-new-virtual-machine)。
    - 删除、 启动、 关闭、 关闭、 暂停、 继续、 重置或重命名虚拟机。 此外将保存虚拟机、 删除已保存的状态，或创建检查点。
    - [更改虚拟机设置](#change-virtual-machine-settings)。
    - 连接到虚拟机控制台通过 HYPER-V 主机使用 VMConnect。
    - [使用 Azure Site Recovery 的虚拟机复制](#protect-virtual-machines-with-azure-site-recovery)。

注意：如果连接到群集，虚拟机工具将仅显示群集的虚拟机。 我们计划在将来也显示非群集虚拟机。

## <a name="create-a-new-virtual-machine"></a>创建新的虚拟机

![创建新的虚拟机屏幕](../media/manage-virtual-machines/new-vm.png)

1. 单击**虚拟机**从左侧导航窗格中的工具。
2. 在虚拟机工具的顶部，选择**清单**选项卡，然后单击**新建**若要创建新的虚拟机。
3. 输入虚拟机名称，第 1 和 2 代虚拟机之间进行选择。
4. 如果要在群集上创建虚拟机，可以选择要首先创建虚拟机上的主机。 如果你正在运行 Windows Server 2016 或更高版本，该工具将为您提供主机的建议。
5. 选择虚拟机文件的路径。 从下拉列表中选择一个卷，或单击**浏览**选择使用文件夹选取器的文件夹。 虚拟机配置文件和虚拟硬盘文件将保存在一个文件夹下`\Hyper-V\\[virtual machine name]`选定的卷或路径的路径。
6. 是否需要启用嵌套虚拟化、 配置内存设置、 网络适配器、 虚拟硬盘和选择是否想要从.iso 映像文件或从网络安装操作系统，请选择虚拟处理器的数。
7. 单击 **“创建”** 创建虚拟机。
8. 虚拟机创建并显示虚拟机列表中，您可以启动虚拟机。
9. 虚拟机启动后，你可以连接到通过 VMConnect 以安装操作系统的虚拟机的控制台。 从列表中选择虚拟机中，单击**更多** > **Connect**下载.rdp 文件。 在远程桌面连接应用中打开.rdp 文件。 由于这连接到虚拟机的控制台，你需要输入的 HYPER-V 主机的管理员凭据。

## <a name="change-virtual-machine-settings"></a>更改虚拟机设置

![虚拟机设置屏幕](../media/manage-virtual-machines/vm-settings.png)

1. 单击**虚拟机**从左侧导航窗格中的工具。
2. 在虚拟机工具的顶部，选择**清单**选项卡。从列表中选择虚拟机，然后单击**更多** > **设置**。
3. 之间进行切换**常规**，**内存**，**处理器**，**磁盘**，**网络**， **启动顺序**并**检查点**选项卡上，配置所需的设置，然后单击**保存**保存当前选项卡的设置。 可用的设置将有所不同根据虚拟机代次。 此外，不能更改某些设置正在运行的虚拟机并将需要首先停止虚拟机。

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>实时迁移到另一个群集节点的虚拟机

如果连接到群集，你可以实时迁移虚拟机移动到另一个群集节点。

1. 在故障转移群集或超聚合群集连接中，单击**虚拟机**从左侧导航窗格中的工具。
2. 在虚拟机工具的顶部，选择**清单**选项卡。从列表中选择虚拟机，然后单击**更多** > **移动**。
3. 从可用的群集节点的列表中选择服务器，然后单击**移动**。
4. 将 Windows Admin Center 右上角显示通知，了解移动进度。 如果移动成功，将看到在虚拟机列表中更改的主机服务器名称。

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>高级的管理和故障排除的单个虚拟机

![单个虚拟机的详细信息屏幕](../media/manage-virtual-machines/vm-details.png)

您可以查看详细的信息和单个虚拟机页中的单个虚拟机的性能图表。

1. 单击**虚拟机**从左侧导航窗格中的工具。
2. 在虚拟机工具的顶部，选择**清单**选项卡。单击虚拟机从虚拟机列表的名称。
3. 在单个虚拟机页上，你可以：
    - 查看虚拟机的详细的信息。
    - 查看实时和历史数据折线图的 CPU、 内存、 网络、 IOPS 和 IO 吞吐量 （历史数据功能仅适用于运行 Windows Server 2019 的超聚合群集）
    - 查看、 创建、 应用、 重命名和删除检查点。
    - 查看虚拟机的虚拟硬盘 (.vhd) 文件、 网络适配器和主机服务器的详细信息。
    - 删除、 启动、 关闭、 关闭、 暂停、 继续、 重置或重命名虚拟机。 此外将保存虚拟机、 删除已保存的状态，或创建检查点。
    - [更改虚拟机设置](#change-virtual-machine-settings)。
    - 连接到虚拟机控制台通过 HYPER-V 主机使用 VMConnect。
    - [将虚拟机使用 Azure Site Recovery 复制](#protect-virtual-machines-with-azure-site-recovery)。

## <a name="view-hyper-v-event-logs"></a>查看 HYPER-V 事件日志

您可以查看 HYPER-V 事件日志直接从虚拟机工具。

1. 单击**虚拟机**从左侧导航窗格中的工具。
2. 在虚拟机工具的顶部，选择**摘要**选项卡。在右上角事件区域中，单击**查看所有事件**。
3. 事件查看器工具将显示在左窗格中的 HYPER-V 事件通道。 选择一个通道，以在右窗格中查看的事件。 如果您正在管理的故障转移群集或超聚合群集，事件日志将显示所有群集节点，在计算机列中显示主机服务器的事件。

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>使用 Azure Site Recovery 保护虚拟机

可以使用 Windows Admin Center 若要配置 Azure Site Recovery 和你的本地虚拟机复制到 Azure。 [了解详细信息](azure-services.md)

## <a name="more-coming"></a>更多

在 Windows Admin Center 中的虚拟机管理正处于开发的主动和将在不久的将来添加新功能。 可以在 UserVoice 中查看状态并进行功能投票：

|功能请求|
|-------|
|[导入/导出虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[排序文件夹中的虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[支持其他虚拟机设置](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[HYPER-V 副本支持](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[委托虚拟机的所有权](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[克隆虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[从现有的虚拟机创建模板](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[查看所有的 HYPER-V 主机的虚拟机](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[配置新的虚拟机窗格中的 VLAN](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**查看所有或提出新功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
