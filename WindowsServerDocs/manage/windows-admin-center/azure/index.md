---
title: 连接到 Azure 混合服务的 Windows Server
description: 你可以通过使用 Azure 混合服务扩展到云的本地部署的 Windows Server。
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: 625bd4b79d277dfaa81767cd781c2ba1316d637e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296885"
---
# 连接到 Azure 混合服务的 Windows Server

>适用于： Windows Server 2019、 Windows Server 2016

你可以通过使用 Azure 混合服务扩展到云的本地部署的 Windows Server。 这些云服务提供有用功能，包括以下的数组：

- 保护虚拟机，并使用 Azure Site Recovery 使用基于云的备份和灾难恢复 (HA/DR)。 
- 跟踪跨应用程序、 网络和基础结构的高级的分析和机器学习 Azure 监视器帮助发生的情况。 
- 简化了网络连接到 Azure 使用 Azure 网络适配器。
- 保持最新的 Azure 更新管理虚拟机。

Azure 混合服务使用以下配置中的 Windows 服务器：

- 独立的物理服务器和虚拟机 (Vm)
- 群集，包括通过[Azure 堆栈 HCI](../../../azure-stack-hci/index.md)和[windows server 软件定义 (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)程序认证的超融合群集

尽管你可以设置使用的 Azure 门户和下载或两个最 Azure 混合服务，许多是直接集成到 Windows Admin Center 提供简化的安装体验，并以服务器为中心的服务的视图。

## Azure 混合服务工具

[Windows Admin Center](../understand/windows-admin-center.md)中的 Azure 混合服务工具将所有集成的 Azure 服务整合到中心，你可以轻松地发现所有可用 Azure 服务，将值引入到你的本地或混合环境。 

![屏幕截图的 Windows Admin Center 显示 Azure 混合服务工具](../media/azure-services/ahs-discover.png)

如果你连接到服务器与 Azure 服务已启用，Azure 混合服务工具可作为单一的以查看该服务器上的所有启用的服务。 你轻松地可以转到 Windows Admin Center 内的相关工具，到更深入地管理这些 Azure 服务或读取多个与文档的 Azure 门户随时启动。 

![屏幕截图的 Windows Admin Center 显示服务器已安装的 Azure 服务](../media/azure-services/ahs-dayN.png)

从 Azure 混合服务工具中，你可以：
- 备份从[Azure 备份](azure-backup.md)与 Windows Admin Center 在 Windows Server
- 从 Windows Admin Center 使用[Azure Site Recovery](azure-site-recovery.md)保护 HYPER-V 虚拟机
- 同步文件服务器与云，使用[Azure 文件同步](azure-file-sync.md)
- 管理操作系统更新所有 Windows 服务器，在本地或在云中使用[Azure 更新管理](azure-update-management.md)
- 监视服务器，在本地或在云中，并使用[Azure 监视器](azure-monitor.md)配置警报
- 将你的本地服务器连接到[Azure 网络适配器](https://aka.ms/WACNetworkAdapter)与 Azure 虚拟网络

## 用于独立服务器和 Vm 的服务

这是提供功能独立服务器和虚拟机的 Azure 服务的完整列表：

- **（新增）通过使用[Azure 文件同步](https://aka.ms/afs)同步与云文件服务器**  
在此服务器与 Azure 文件共享上的文件同步。 将所有文件都保留在本地或使用云分层和缓存仅最常使用的分层到云的冷数据在服务器上的文件。 在云中的数据可以备份，无需担心在本地服务器备份。 此外，多站点同步可保持一组文件同步跨多个服务器。

- **将一层安全添加到 Windows Admin Center，通过添加[Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)身份验证**  
通过要求用户进行身份验证使用 Azure Active Directory (Azure AD) 标识来访问该网关，你可以添加到 Windows Admin Center 一层额外的安全。 Azure AD 身份验证，还可以充分利用 Azure AD 的安全功能，如条件访问和多重身份验证。  
有关详细信息，请参阅[配置 Azure AD 身份验证 Windows Admin center。](../configure/user-access-control.md#azure-active-directory)  

- **保护 HYPER-V 虚拟机使用[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
你可以复制，以便你的业务关键基础结构受到保护出现灾难，虚拟机上运行的工作负荷。 Windows Admin Center 简化了设置和复制虚拟机 HYPER-V 服务器或群集上的处理让你更轻松地支持与 Azure Site Recovery 灾难恢复服务环境的复原。  
有关详细信息，请参阅[保护虚拟机使用 Azure Site Recovery 和 Windows Admin Center](azure-site-recovery.md)。

- **备份你的 Windows 服务器与[Azure 备份](https://docs.microsoft.com/azure/backup/backup-overview)**  
可以将 Windows server 备份到 Azure 后，帮助保护你免受意外或恶意删除、 损坏和勒索软件。  
有关详细信息，请参阅[备份你的服务器与 Azure 备份](azure-backup.md)。

- **监视和可以针对[虚拟机的 Azure 监视](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)与你的环境中的所有服务器获取电子邮件警报**  
你可以使用 Azure 监视，也称为虚拟机见解，监视服务器运行状况和事件、 创建电子邮件通知，在你的环境中获取服务器性能的统一的视图和可视化应用，系统，并且服务连接到给定服务器。  
有关详细信息，请参阅[连接到 Azure 监视器服务器和配置电子邮件通知](azure-monitor.md)。

- **使用[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)的所有 Windows 服务器来集中都管理操作系统更新**  
从一个位置，而不是在每台服务器上，你可以管理更新和多个服务器和 Vm 修补程序。 使用 Azure 更新管理，您可以快速评估可用更新的状态、 计划安装所需的更新，并查看部署结果，以验证已成功应用更新。 这是可能你的服务器是否 Azure 虚拟机，托管由其他云提供程序，或在本地。  
有关详细信息，请参阅[Azure 更新管理配置服务器](azure-update-management.md)。

- **将你的本地服务器连接到[Azure 网络适配器](https://aka.ms/WACNetworkAdapter)与 Azure 虚拟网络**  
你可以添加到你的本地服务器来帮助你安全地将服务器连接到 Azure 虚拟网络的 Azure 网络适配器。  
有关详细信息，请参阅[在本地 Windows Server 和 Azure 虚拟网络之间配置点到站点 VPN 连接](https://aka.ms/WACNetworkAdapter)。

- **管理 Azure IaaS 虚拟机使用[Windows Admin Center](manage-azure-vms.md)**  
你可以使用 Windows Admin Center 管理 Azure 虚拟机以及在本地计算机。 通过配置你的 Windows Admin Center 网关连接到 Azure VNet，你可以管理中使用 Windows Admin Center 提供的一致、 简化工具的 Azure 虚拟机。 有关详细信息，请参阅[配置 Windows Admin Center 管理 Azure 中的虚拟机](manage-azure-vms.md)。

## 服务的群集

这些是提供群集功能作为一个整体的 Azure 服务 （这些不所有集成到 Windows Admin Center 尚未）：

- [超聚合群集的监视器上使用 Azure 监视器](../../../storage/storage-spaces/configure-azure-monitor.md)
- [保护虚拟机使用 Azure Site Recovery](azure-site-recovery.md)
- [部署群集云见证](../../../failover-clustering/deploy-cloud-witness.md)

## 另请参阅

- [将 Windows Admin Center 连接到 Azure](azure-integration.md)
- [部署 Azure 中的 Windows Admin Center](deploy-wac-in-azure.md)