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

Azure 混合服务使用下列配置中的 Windows 服务器：

- 独立的物理服务器和虚拟机 (VM)
- 群集，包括通过了 [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) 认证的超融合群集，以及 [Windows Server 软件定义 (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter) 程序

虽然您可以设置为大多数 Azure 混合服务使用 Azure 门户和下载或两个，但许多都直接集成到 Windows Admin Center，以提供简化的安装程序体验和服务的以服务器为中心视图。

## <a name="azure-hybrid-services-tool"></a>Azure 混合服务工具

Azure 混合服务工具中[Windows Admin Center](../understand/windows-admin-center.md)将所有集成的 Azure 服务整合到一个集中式中心可以轻松地发现所有可用 Azure 服务以使值到你的本地或混合环境。 

![屏幕截图的 Windows Admin Center 显示 Azure 混合服务工具](../media/azure-services/ahs-discover.png)

如果已启用的 Azure 服务连接到服务器，Azure 混合服务工具可作为单个窗格来查看该服务器上的所有已启用的服务。 可以轻松地转到 Windows Admin Center 中的相关工具、 到 Azure 门户，了解这些 Azure 服务或阅读更多的文档的更深入地管理你可以轻松启动。 

![屏幕截图的 Windows Admin Center 显示已安装在服务器的 Azure 服务](../media/azure-services/ahs-dayN.png)

从 Azure 混合服务工具，您可以：
- 备份 Windows Server 与 Windows Admin Center 从[Azure 备份](azure-backup.md)
- 从 Windows Admin Center 与保护的 HYPER-V 虚拟机[Azure 站点恢复](azure-site-recovery.md)
- 同步文件服务器与在云中使用[Azure 文件同步](azure-file-sync.md)
- 管理操作系统更新的所有 Windows 服务器，同时在本地或云中，与[Azure 更新管理](azure-update-management.md)
- 监视服务器，同时在本地或云中，并配置警报与[Azure Monitor](azure-monitor.md)
- 将你的本地服务器连接到 Azure 虚拟网络与[Azure 网络适配器](https://aka.ms/WACNetworkAdapter)

## <a name="services-for-stand-alone-servers-and-vms"></a>服务的独立服务器和 Vm

这是提供的独立服务器和 Vm 的功能的 Azure 服务的完整列表：

- **（新）使用同步文件服务器与云[Azure 文件同步](https://aka.ms/afs)**  
使用 Azure 文件共享此服务器上的同步文件。 将所有文件都保留在本地或使用云分层和缓存仅最常用的冷数据分层到云中的服务器上的文件。 云中的数据可以进行备份，无需担心如何在本地服务器备份。 此外，多站点同步可让一组文件保持同步跨多个服务器。

- **通过添加到 Windows Admin Center 添加一层安全[Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)身份验证**  
通过要求用户使用 Azure Active Directory (Azure AD) 标识访问网关进行身份验证，可以向 Windows Admin Center 添加额外的安全层。 Azure AD 身份验证还允许您充分利用 Azure AD 的安全功能，如条件性访问和多重身份验证。  
有关详细信息，请参阅[Windows Admin Center 配置 Azure AD 身份验证。](../configure/user-access-control.md#azure-active-directory)  

- **保护 HYPER-V 虚拟机与[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
可以复制在 Vm 上运行，以便在发生灾难时保护您的业务关键基础结构的工作负荷。 Windows Admin Center 简化了安装程序并将复制的 HYPER-V 服务器或群集的虚拟机的过程更轻松地还能稳固你的环境与 Azure Site Recovery 的灾难恢复服务的复原能力。  
有关详细信息，请参阅[保护 Vm 与 Azure Site Recovery 和 Windows Admin Center](azure-site-recovery.md)。

- **备份 Windows 服务器与[Azure 备份](https://docs.microsoft.com/azure/backup/backup-overview)**  
可以将 Windows 服务器备份到 Azure，从而帮助防止意外或恶意删除、 损坏和勒索软件。  
有关详细信息，请参阅[备份使用 Azure 备份服务器](azure-backup.md)。

- **监视和获取针对您的环境中的所有服务器的电子邮件警报[虚拟机的 Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Azure Monitor，也称为虚拟机见解，可用于监视服务器运行状况和事件、 创建电子邮件警报、 跨您的环境中获取的服务器性能的合并的视图和可视化应用程序，系统和服务连接到给定服务器。  
有关详细信息，请参阅[将服务器连接到 Azure Monitor 和配置电子邮件通知](azure-monitor.md)。

- **集中管理的所有 Windows 服务器的操作系统更新[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)**  
你可以从单个位置，而不是在每台服务器上管理更新和修补程序的多个服务器和 Vm。 借助 Azure 更新管理，可以快速评估可用更新的状态、 计划所需更新的安装和查看部署结果，验证已成功应用的更新。 这是可能你的服务器是否是由其他云提供商托管的 Azure Vm，或在本地。  
有关详细信息，请参阅[为 Azure 更新管理配置服务器](azure-update-management.md)。

- **将你的本地服务器连接到 Azure 虚拟网络与[Azure 网络适配器](https://aka.ms/WACNetworkAdapter)**  
可以将 Azure 网络适配器添加到您的本地服务器，以帮助你安全地将服务器连接到 Azure 虚拟网络。  
有关详细信息，请参阅[配置点到站点 VPN 连接的本地 Windows Server 和 Azure 虚拟网络之间](https://aka.ms/WACNetworkAdapter)。

- **管理 Azure IaaS 虚拟机与[Windows Admin Center](manage-azure-vms.md)**  
可以使用 Windows Admin Center 来管理 Azure Vm 以及本地计算机上。 通过配置 Windows Admin Center 网关连接到 Azure VNet，可以管理虚拟机在 Azure 中使用 Windows Admin Center 提供的一致、 简化工具。 有关详细信息，请参阅[配置 Windows Admin Center 来管理在 Azure 中的虚拟机](manage-azure-vms.md)。

## <a name="services-for-clusters"></a>群集服务

这些是提供作为一个整体功能到群集的 Azure 服务 （这些不所有集成到 Windows Admin Center 尚未）：

- [使用 Azure Monitor 监视超聚合群集](../../../storage/storage-spaces/configure-azure-monitor.md)
- [使用 Azure Site Recovery 对 Vm 进行保护](azure-site-recovery.md)
- [部署群集云见证](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>另请参阅

- [Windows Admin Center 连接到 Azure](azure-integration.md)
- [部署在 Azure 中 Windows Admin Center](deploy-wac-in-azure.md)