---
title: Azure Site Recovery 服务集成
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/01/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 94894cd9be06485bf842f96517172db709dbe93d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848738"
---
#<a name="azure-site-recovery-services-integration"></a>Azure Site Recovery 服务集成

>适用于：Windows Server 2016 Essentials

[Azure Site Recovery 服务](https://docs.microsoft.com/azure/site-recovery/)到 Azure 的备份保管库是由启用的虚拟机 (VM) 的实时复制的 Microsoft Azure 提供的服务。 在的服务器或站点已关闭由于硬件或其他故障，你可以故障转移到 Azure 备份保管库中存储的 VM 映像位置设置为在 Azure 中正在运行的 VM。 与 Azure 虚拟网络，发生故障转移到 Azure，结合使用客户端以前连接到的本地服务器的 Pc 将以透明方式连接到在 Azure 中运行的服务器中。

将与 Windows Server Essentials 集成 Azure Site Recovery 服务在与配置相同的方式启动[Azure 虚拟网络](azure-virtual-network-integration.md)。 从**Microsoft 云服务集成**在仪表板页上，单击**与 Azure Site Recovery 服务集成**右侧的仪表板：

![Windows Server Essentials 仪表板的主页上显示开始选项卡屏幕截图。 在开始选项卡上，已选择服务部分，和仪表板的 Microsoft 云服务集成下方，指示 Azure 恢复当前已禁用。](media/azure-site-recovery-1.PNG)

使用 Azure 虚拟网络集成和 Azure 备份集成，必须使用你现有的 Azure 帐户，登录到 Azure，或创建新的帐户：

![显示启用到 Azure 的复制向导的登录到 Microsoft Azure 页屏幕截图。 显示登录按钮，因为用户具有尚未登录到 Microsoft Azure。](media/azure-site-recovery-2.PNG)

后成功登录到 Azure，您会看到一个屏幕，询问您要将与 Azure Site Recovery 服务，以及将存储和托管你的 VM 在 Azure 区域相关联的订阅：

![显示启用到 Azure 的复制向导的登录到 Microsoft Azure 页屏幕截图。 因为用户已登录到 Microsoft Azure 中，此页提供了用于选择订阅、 存储帐户和区域的选项。](media/azure-site-recovery-3.PNG)

订阅和区域选择之后, 的新选项卡将显示在**Windows Server Essentials 仪表板**称为**Azure Recovery**。 网络扫描是为了确定并枚举受支持的主机服务器 (运行 Windows Server HYPER-V 2012 R2 及更高版本) 下的各个主机的虚拟机 （来宾） 以及：

![显示 Azure Recovery 页 Windows Server Essentials 仪表板的屏幕截图。 两个 HYPER-V 主机以及这些主机上运行的虚拟机显示。 为此虚拟机当前已禁用名为 ramh157v01 选择 RAM-h 1-7，则的主机上的虚拟机和复制到 Azure。](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>启用来宾虚拟机保护

你可以单击虚拟机存在 Azure Recovery 窗口中选择它后**启用到 Azure 的复制**准备并复制虚拟机的仪表板右侧™ s 到 Azure 的映像：

![显示启用复制到 Azure 对话框屏幕截图。 正在添加主机时，显示一个进度栏。](media/azure-site-recovery-5.PNG)

在此过程中，Azure Site Recovery 服务代理安装在主机服务器上，备份保管库，其中创建的来宾操作系统将存储虚拟机映像，并开始将复制到 Azure 的映像。 根据要复制的 VM 的大小，复制过程完成可能需要数小时或几天。 直到整个 VM 映像和最新的增量复制到 Azure，故障转移任务不可用而未受保护 VM。 完成复制后，Azure Recovery 窗口中的保护状态列将从**复制**到**已启用**:

![显示 Azure Recovery 页 Windows Server Essentials 仪表板的屏幕截图。 两个 HYPER-V 主机以及这些主机上运行的虚拟机显示。 当前为此虚拟机启用名为 ramh12v02 选择 RAM-h 1-2，主机上的虚拟机和复制到 Azure。](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>来宾 VM 到 Azure 的故障转移

![显示 VM 的故障转移到 Azure 的页的屏幕截图。](media/azure-site-recovery-7.PNG)

虚拟机的受保护的失败或主机服务器上失败，故障转移到 Azure 的受保护的虚拟机运行可完成以保持业务连续性，直到本地虚拟机或主机服务器时，修复，并且已可用。 作为该图中所示，有三种类型的故障转移，也可通过 Azure Site Recovery 服务支持：

-   **测试故障转移**ƒA 良好的灾难恢复计划包含的功能来模拟在发生灾难，以确保实际灾难发生时的停机时间最短。 测试故障转移将已复制到恢复保管库、 设置为在 Azure 中运行的虚拟机并使你能够连接到服务器以测试适用于企业的方案的 VM 映像。 测试故障转移期间，本地虚拟机继续运行并在灾难恢复测试期间不会中断业务的任何中断。 在测试故障转移完成后，通过 Azure 门户中，后者将取消预配虚拟机并删除将 VHD 测试将停止。 整个测试故障转移期间，将继续恢复保管库中的 VM 映像，以获取已复制从现场 VM 像曾经发生在执行任何操作。

-   **计划外故障转移**ƒAn 与受保护的主机服务器或主机服务器上运行的虚拟机的实际错误时，会发生计划外故障转移。 在故障转移是 Windows Server Essentials 仪表板中，手动触发或发生故障的服务器本身，运行 Windows Server Essentials 的服务器是否可以触发从 Azure 门户直接。 在这种情况下，计划外故障转移花费的已复制到恢复保管库、 设置为在 Azure 中运行的虚拟机并使你能够连接到服务器以测试适用于企业的方案的 VM 映像。 当在本地恢复你的服务器时，可以从 Azure 门户，然后将复制到本地服务器的 VM 映像触发手动故障回复。

-   **计划的故障转移**ƒA 计划的故障转移是在的计划内的活动，如硬件维护，必须进行这会向下使服务器可以执行的操作。 发生计划内故障转移时，相同的过程进行方面的已复制的 VM 映像在 Azure 中预配。 但是，在这样做之前, 在本地服务器关闭有条理地以确保所有更改都复制到 Azure 之前关闭。 在计划内的维护完成后，可以手动触发故障回复从 Windows Server Essentials 仪表板或 Azure 门户中，这会启动本地服务器并在 Azure 中，删除 VM，取消其预配。VHD 文件。 从本地 VM 复制到 Azure 将继续再次发生正常。

在任何上述三种情况下，当 VM 故障转移到 Azure，Windows Server Essentials 仪表板将显示新的虚拟机中运行如下图所示的 Azure。

![显示 Azure Recovery 页 Windows Server Essentials 仪表板的屏幕截图。 对于名为一些基础知识，以及在 Azure 中运行的名为的 Essentials-Test 指示主机已故障转移到 Azure 的虚拟机的主机已启用复制到 Azure。](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>请参阅
--------
[开始使用 Windows Server Essentials](get-started.md)
