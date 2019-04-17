---
title: "Azure 网站恢复服务集成"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 85e681cfc19241941773dd94f05ba59759aaf62a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
#<a name="azure-site-recovery-services-integration"></a>Azure 网站恢复服务集成

>适用于：Windows Server 2016 软件包

[Azure 网站恢复服务](https://azure.microsoft.com/en-us/documentation/services/site-recovery/)对在 Azure 备份圆顶是由 Microsoft Azure 虚拟机 (VM) 的实时复制的启用提供服务。 在服务器或站点已关闭由于硬件或其他故障，你可以故障转移到 Azure 了 VM 映像存储在你备份的圆顶也将为运行在 Azure 虚拟机预配。 结合使用 Azure 虚拟网络，如果故障转移到 Azure，客户端之前将其连接到本地服务器的 Pc 将坦诚将连接到在 Azure 上运行的服务器。

与 Windows Server Essentials 集成 Azure 网站恢复服务的方式相同配置启动[Azure 虚拟网络](azure-virtual-network-integration.md)。 从**Microsoft 云服务集成**仪表板中的页面上，单击**与 Azure 站点恢复服务集成**仪表板右侧：

![在主页上的 Windows Server Essentials 仪表板中显示的开始选项卡的屏幕截图。 在开始选项卡上，已选择服务部分，，并且在 Microsoft 云服务集成指示仪表板 Azure 恢复当前已禁用。](media/azure-site-recovery-1.PNG)

使用 Azure 虚拟网络的集成 Azure 备份集成，必须登录到 Azure 与你的现有 Azure 帐户，或创建一个新帐户：

![显示给 Azure 启用复制向导登录中向 Microsoft Azure 页面的屏幕截图。 因为用户尚未签署 Microsoft Azure 到显示了登录按钮。](media/azure-site-recovery-2.PNG)

在成功登录 Azure 之后, 你将看到会询问你想要将与 Azure 站点恢复服务，以及将存储和托管你 VM Azure 地区关联的订阅的屏幕：

![显示给 Azure 启用复制向导登录中向 Microsoft Azure 页面的屏幕截图。 用户已登录到 Microsoft Azure，因为此页面将提供选项来选择订阅、 存储帐户和区域。](media/azure-site-recovery-3.PNG)

订阅和选择地区之后, 将显示一个新选项卡在**Windows Server Essentials 仪表板**称为**Azure 恢复**。 网络扫描完成识别和枚举受支持的主服务器 (运行 Windows Server HYPER-V 2012 R2 或更) 以及下的单个主机虚拟机 （来宾）：

![显示 Windows Server Essentials 仪表板 Azure 恢复页面的屏幕截图。 两个 HYPER-V 主机将显示虚拟机运行这些主机以及。 此虚拟机当前禁用名为选择 RAM-H1 7，则的主机上 ramh157v01 虚拟机和复制到 Azure。](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>启用来宾虚拟机保护

根据存在 Azure 恢复窗口在虚拟机的选择，你可以单击**启用的复制到 Azure**仪表板准备并复制虚拟机右侧™ 到 Azure 的映像：

![屏幕截图显示了对 Azure 启用复制对话框。 添加主机进度栏将显示。](media/azure-site-recovery-5.PNG)

在此过程中，在主服务器上，位置创建将存储 VM 访客的图像，并复制到 Azure 图像的开始备份圆顶安装 Azure 站点恢复服务代理。 根据 VM 复制的大小，完成复制过程可能需要几小时或几天。 整个 VM 图像和新增量复制到 Azure，直到故障转移任务不可用，VM 不受保护。 Azure 恢复窗口保护状态栏复制完毕后，将从**复制**到**启用**:

![显示 Windows Server Essentials 仪表板 Azure 恢复页面的屏幕截图。 两个 HYPER-V 主机将显示虚拟机运行这些主机以及。 当前已名为选择 RAM-H1-2，则的主机上 ramh12v02 虚拟机，并复制到 Azure 启用此虚拟机。](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>进行故障转移到 Azure 虚拟机的访客

![显示 VM 故障转移到 Azure 页面的屏幕截图。](media/azure-site-recovery-7.PNG)

当虚拟机的受保护的失败，或者可以进行失败，失败转移到 Azure 上运行的受保护的虚拟机维护直到业务连续性主服务器本地虚拟机或主服务器上是修复，并且可用。 作为上方节目图中，有三种类型的故障转移受支持的 Azure 网站恢复服务：

-   **测试故障转移**ƒA 好灾难恢复套餐包含模拟灾难以确保降至最低停机真实灾难的功能。 测试故障转移拍摄 VM 张图像已复制到你的恢复圆顶，它设置为在 Azure 虚拟机运行并使您能够连接到服务器测试适用于企业的方案。 测试故障转移期间，本地虚拟机将继续运行，并且不会业务打乱灾难恢复测试期间没有中断与。 完成测试故障转移后，你将通过消除配置虚拟机和删除 VHD 的 Azure 门户测试停止。 整个测试故障转移期间，在你的恢复圆顶 VM 图像继续获取从现场 VM 复制，就像任何有史以来发生了变化。

-   **计划的故障转移**ƒAn 计划的故障转移时发生实际故障的受保护的主服务器或 VM 主服务器上运行。 在故障转移手动触发任一 Windows Server Essentials 仪表板中，从或运行 Windows Server Essentials 服务器失败的服务器本身是否可以触发从 Azure 门户直接。 在此情况下，计划的故障转移采用 VM 图像已复制到你的恢复圆顶，它设置为在 Azure，运行虚拟机，并使您能够连接到服务器测试适用于企业的方案。 在本地还原服务器后，可以从 Azure 门户，然后将复制回下本地服务器 VM 图像触发手动故障回复。

-   **计划故障转移**计划 ƒA 故障转移，则可以在该计划的活动，如硬件维护必须发生这会花费服务器下采取的操作。 如果计划的故障转移的同一进程发生在的在 Azure 你复制的 VM 映像预配方面。 但是，此修复中之前, 本地服务器关闭中有序的方式以确保在关闭之前复制到 Azure 所有更改。 计划的维护完毕后，可以手动触发故障从 Windows Server Essentials 仪表板中或 Azure 门户，这会提出本地服务器，然后消除配置 Azure 和删除 VM 回复。VHD 文件。 Azure 到本地 VM 复制然后继续发生再次正常。

在上方的三种情况下，当 VM 故障转移到 Azure，Windows Server Essentials 仪表板将显示的新虚拟机 Azure 运行如下所示下图中。

![显示 Windows Server Essentials 仪表板 Azure 恢复页面的屏幕截图。 复制到 Azure 名为 Essentials，并在 Azure 上运行的命名的软件包-测试指示主机了故障转移到 Azure 虚拟机的主机已启用。](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>请参阅
--------
[Windows Server Essentials 入门](get-started.md)
