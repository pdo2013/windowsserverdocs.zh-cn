---
title: 将 Windows Server 连接到 Azure 混合服务
description: 可以通过使用 Azure 混合服务将 Windows Server 的本地部署扩展到云。
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/31/2019
ms.openlocfilehash: 460399a57bc229b44d37a9fdd1e4938bf9e7d6ac
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455360"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>将 Windows Server 连接到 Azure 混合服务

>适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

可以通过使用 Azure 混合服务将 Windows Server 的本地部署扩展到云。 这些云服务提供了一系列有用的功能，其中包括：

- 保护虚拟机，并且将基于云的备份和灾难恢复 (HA/DR) 与 Azure Site Recovery 配合使用。 
- 借助 Azure Monitor 中的高级分析与机器学习，跨应用程序、网络和基础结构跟踪发生的事件。 
- 通过 Azure 网络适配器简化了与 Azure 的网络连接。
- 通过 Azure 更新管理将虚拟机保持最新。

Azure 混合服务在下列配置中可与 Windows Server 配合使用：

- 独立的物理服务器和虚拟机 (VM)
- 群集，包括通过了 [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) 认证的超融合群集，以及 [Windows Server 软件定义 (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter) 程序

虽然可以使用 Azure 门户和一两个下载项安装大多数 Azure 混合服务，但许多 Azure 混合服务都直接集成到 Windows Admin Center 中，以便提供简化的安装体验和以服务器为中心的服务视图。

## <a name="azure-hybrid-services-tool"></a>Azure 混合服务工具

[Windows Admin Center](../understand/windows-admin-center.md) 中的 Azure 混合服务工具将所有集成的 Azure 服务整合到一个集中式中心，你在其中可以轻松地发现可为本地或混合环境带来价值的所有可用 Azure 服务。 

![显示 Azure 混合服务工具的 Windows Admin Center 屏幕截图](../media/azure-services/ahs-discover.png)

如果在已启用 Azure 服务的情况下连接到服务器，Azure 混合服务工具可充当一片玻璃，供你查看该服务器上的所有已启用服务。 可以在 Windows Admin Center 中轻松访问相关工具，导航到 Azure 门户以便更深入地管理这些 Azure 服务，或者通过手头的文档了解详情。 

![显示服务器上已安装的 Azure 服务的 Windows Admin Center 屏幕截图](../media/azure-services/ahs-dayN.png)

从 Azure 混合服务工具中可以：
- 使用 [Azure 备份](azure-backup.md)从 Windows Admin Center 中备份 Windows Server
- 使用 [Azure Site Recovery](azure-site-recovery.md) 从 Windows Admin Center 中保护 Hyper-V 虚拟机
- 使用 [Azure 文件同步](azure-file-sync.md)将文件服务器与云同步
- 使用 [Azure 更新管理](azure-update-management.md)管理所有 Windows 服务器（在本地或在云中）的操作系统更新
- 使用 [Azure Monitor](azure-monitor.md) 监视服务器（在本地或在云中）并配置警报
- 使用 [Azure 网络适配器](https://aka.ms/WACNetworkAdapter)将本地服务器连接到 Azure 虚拟网络

## <a name="services-for-stand-alone-servers-and-vms"></a>适用于独立服务器和 VM 的服务

这是为独立服务器和 VM 提供功能的 Azure 服务的完整列表：

- **（新）使用 [Azure 文件同步](https://aka.ms/afs)将文件服务器与云同步**  
将此服务器上的文件与 Azure 文件共享同步。 将所有文件都保留在本地，或者使用云分层并且仅缓存服务器上最常用的文件，将冷数据分层到云中。 云中的数据可以备份，这样就无需担心本地服务器备份。 此外，多站点同步可让一组文件在多个服务器上保持同步。

- **通过添加 [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) 身份验证向 Windows Admin Center 添加安全层**  
可以通过要求用户使用 Azure Active Directory (Azure AD) 标识访问网关进行身份验证，向 Windows Admin Center 添加额外的安全层。 使用 Azure AD 身份验证，还可以充分利用 Azure AD 的安全功能，如条件访问和多重身份验证。  
有关详细信息，请参阅[为 Windows Admin Center 配置 Azure AD 身份验证。](../configure/user-access-control.md#azure-active-directory)  

- **使用 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) 保护 Hyper-V 虚拟机**  
可以复制在虚拟机上运行的工作负载，以便在出现灾难时能够让你的业务关键基础结构受到保护。 Windows Admin Center 简化了安装以及将虚拟机复制到 HYPER-V 服务器或群集的过程，让你可以更轻松地通过 Azure Site Recovery 的灾难恢复服务提升环境的复原能力。  
有关详细信息，请参阅[使用 Azure Site Recovery 和 Windows Admin Center 保护 VM](azure-site-recovery.md)。

- **使用 [Azure 备份](https://docs.microsoft.com/azure/backup/backup-overview)来备份 Windows 服务器**  
可以将 Windows 服务器备份到 Azure，从而帮助防止意外或恶意删除、损坏和勒索软件侵害。  
有关详细信息，请参阅[使用 Azure 备份来备份服务器](azure-backup.md)。

- **使用[用于虚拟机的 Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) 监视环境中的所有服务器并获取相关电子邮件警报**  
Azure Monitor（也称为虚拟机见解）可用于监视服务器运行状况和事件、创建电子邮件警报、获取整个环境中服务器性能的合并视图以及将连接到给定服务器的应用、系统和服务可视化。  
有关详细信息，请参阅[将服务器连接到 Azure Monitor 并配置电子邮件通知](azure-monitor.md)。

- **使用 [Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)集中管理所有 Windows 服务器的操作系统更新**  
可以从单个位置（而不是在每个服务器上）管理多个服务器和 VM 的更新和修补程序。 借助 Azure 更新管理，可以快速评估可用更新的状态、计划所需更新的安装，以及查看部署结果来验证更新是否已成功应用。 无论服务器是 Azure VM、由其他云提供商托管或是本地服务器，均可如此。  
有关详细信息，请参阅[为 Azure 更新管理配置服务器](azure-update-management.md)。

- **使用 [Azure 网络适配器](https://aka.ms/WACNetworkAdapter)将本地服务器连接到 Azure 虚拟网络**  
可以将 Azure 网络适配器添加到本地服务器，从而帮助你安全地将服务器连接到 Azure 虚拟网络。  
有关详细信息，请参阅[配置本地 Windows Server 和 Azure 虚拟网络之间的点到站点 VPN 连接](https://aka.ms/WACNetworkAdapter)。

- **使用 [Windows Admin Center](manage-azure-vms.md) 管理 Azure IaaS 虚拟机**  
可以使用 Windows Admin Center 来管理 Azure VM 以及本地计算机。 通过将 Windows Admin Center 网关配置为连接到 Azure VNet，可以使用 Windows Admin Center 提供的一致、简化的工具在 Azure 中管理虚拟机。 有关详细信息，请参阅[配置 Windows Admin Center 以便在 Azure 中管理 VM](manage-azure-vms.md)。

## <a name="services-for-clusters"></a>适用于群集的服务

这些是作为一个整体为群集提供功能的 Azure 服务（这些并非均已集成到 Windows Admin Center）：

- [使用 Azure Monitor 监视超融合群集](../../../storage/storage-spaces/configure-azure-monitor.md)
- [使用 Azure Site Recovery 保护 VM](azure-site-recovery.md)
- [部署群集云见证](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>另请参阅

- [将 Windows Admin Center 连接到 Azure](azure-integration.md)
- [在 Azure 中部署 Windows Admin Center](deploy-wac-in-azure.md)