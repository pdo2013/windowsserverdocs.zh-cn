---
title: Microsoft Hyper-V Server 2016
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: KBDAzure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: 9a1b6ea7b9abc94f63a1390b6fa18e4c8d4a1822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839368"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft HYPER-V Server 2016 是独立\-单独的产品，包含仅在 Windows 虚拟机监控程序、 Windows Server 驱动程序模型和虚拟化组件。 它提供简单且可靠的虚拟化解决方案，以帮助您提高服务器利用率并降低成本。

Microsoft HYPER-V Server 2016 中的 Windows 虚拟机监控程序技术等同于中的 Hyper\-V 角色 Windows Server 2016 上的。 因此，很多内容可用于超\-V 角色 Windows Server 2016 上的也适用于 Microsoft HYPER-V Server 2016。

## <a name="hyper-v-server-resources-for-it-pros"></a>超\-面向 IT 专业人员 V Server 资源

|任务|资源|
|-|-|
|![满足要求符号](media/All_Symbols_MeetsRequirements.png)|**评估的 HYPER-V**<br /><br />-   [HYPER-V 技术概述](hyper-v-technology-overview.md)<br />- [什么是 Windows Server 2016 上的 HYPER-V 中的新增功能](what-s-new-in-hyper-v-on-windows.md)<br />-   [Windows Server 2016 上的 HYPER-V 的系统要求](system-requirements-for-hyper-v-on-windows.md)<br />-   [支持的 Windows 来宾操作系统的 HYPER-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [受支持的 Linux 和 FreeBSD 虚拟机](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   [生成和来宾的功能兼容性](hyper-v-feature-compatibility-by-generation-and-guest.md)<br /><br />**适用于 HYPER-V 的计划**<br /><br />-确定哪个[虚拟机代次](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md)满足你的需求。 <br/>-如果您要移动或导入虚拟机，决定何时[升级版本](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。 <br />- [可伸缩性](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [网络](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [安全](plan/plan-hyper-v-security-in-windows-server.md)|
|![获取已启动的符号](media/All_Symbols_GetStarted.png)|**HYPER-V Server 入门**<br /><br />[下载并安装 Microsoft 超\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016)。 这将安装在 Windows 虚拟机监控程序、 Windows Server 驱动程序模型和虚拟化组件。 它相当于运行服务器核心安装选项的 Windows Server 2016 和超\-V 角色。|
|![管理符号](media/All_Symbols_Administrator.png)|**配置和管理 HYPER-V 服务器**<br /><br />超\-V 服务器没有图形用户界面\(GUI\)。 可以使用以下工具来配置和管理超\-V 服务器。<br /><br />-   [通过 Sconfig.cmd 配置 Windows Server 2016 的服务器核心安装](../../get-started/sconfig-on-ws2016.md)更新域或工作组设置，更改 Windows 更新设置、 启用远程管理的详细信息。<br />-使用普通[命令提示符下](../../administration/windows-commands/windows-commands.md)命令在 Sconfig 中不可用。<br />-使用[超\-管理器](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management)或[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm)远程管理超\-V 服务器。 若要使用超\-V 管理器中，[安装超\-V 角色在 Windows 10 上的](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)或[Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md)。<br />-请参阅[安装服务器核心](../../get-started/getting-started-with-server-core.md)有关附加的管理选项的服务器的基本功能并非特定于超\-V。 大多数那里记录的管理方法也适用于超\-V 服务器。<br /><br />**配置和管理超\-V 虚拟机**<br /><br />-   [创建的虚拟交换机的 HYPER-V 虚拟机。](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [在 HYPER-V 中创建的虚拟机](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [标准或生产检查点之间选择](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [启用或禁用检查点](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [使用 PowerShell Direct 管理 Windows 虚拟机](manage/manage-windows-virtual-machines-with-powershell-direct.md) <br /><br />**部署**<br /><br />-   [设置为没有故障转移群集的实时迁移的主机](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [升级 Windows Server 群集节点](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [升级虚拟机版本](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
