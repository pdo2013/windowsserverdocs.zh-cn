---
title: 启用 RDS 使用 Azure Site Recovery 的灾难恢复
description: 了解如何启用 RDS 使用 Azure Site Recovery 的灾难恢复。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: e3f9db4afb37452b4fd5d0229b385492b915fe45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859008"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>启用 RDS 使用 Azure Site Recovery 的灾难恢复

>适用于：Windows 服务器 （半年频道），Windows Server 2016

若要确保充分地进行灾难恢复配置 RDS 部署，需要保护所有构成了 RDS 部署的组件：

- Active Directory
- SQL Server 层
- RDS 组件
- 网络组件
 
## <a name="configure-active-directory-and-dns-replication"></a>配置 Active Directory 和 DNS 复制

你需要 RDS 部署在灾难恢复站点上的 Active Directory 工作。 必须根据 RDS 部署的复杂程度的两种选择：

- 选项 1-如果有少量的应用程序和单个域控制器的整个本地站点，并且需要故障转移整个站点进行，使用 ASR 复制域控制器复制到辅助站点 (都为 true站点到站点和站点到 Azure 方案）。
- 选项 2-如果有大量应用程序和正在运行 Active Directory 林，并且你将故障转移少量的应用程序一次设置灾难恢复站点上的其他域控制器 (辅助站点或 Azure 中)。

请参阅[保护 Active Directory 和 DNS 与 Azure Site Recovery 配合使用](/azure/site-recovery/site-recovery-active-directory)有关使域控制器在灾难恢复站点上可用的详细信息。 本指南的其余部分，我们假定您已按照这些步骤，并且有可用的域控制器。

## <a name="set-up-sql-server-replication"></a>设置 SQL Server 复制

请参阅[使用 SQL Server 灾难恢复和 Azure Site Recovery 保护 SQL Server](/azure/site-recovery/site-recovery-sql)有关将 SQL Server 复制设置的步骤。

## <a name="enable-protection-for-the-rds-application-components"></a>启用 RDS 应用程序组件的保护

具体取决于您的 RDS 部署类型，可以为 Vm 启用保护不同组件 （如在下表中列出） 在 Azure Site Recovery 中。 配置相关的 Azure Site Recovery 元素基于是否已将 Vm 部署在 HYPER-V 或 VMWare 上。

| 部署类型                              | 保护的步骤                                                                                                                                                                                      |
|----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 个人虚拟桌面 （非托管）         |  1.请确保所有虚拟化主机都安装了 RDVH 角色准备就绪。    </br>2.连接代理。  </br>3.个人桌面。 </br>4.黄金模板 VM。 </br>5.Web 访问，许可证服务器和网关服务器 |
| 共用虚拟桌面 （托管与没有 UPD） |  1.所有虚拟化主机已准备安装了 RDVH 角色。  </br>2.连接代理。  </br>3.黄金模板 VM。 </br>4.Web 访问，许可证服务器和网关服务器。                                  |
| Remoteapp 和桌面会话 (没有 UPD)     |  1.会话主机。  </br>2.连接代理。 </br>3.Web 访问，许可证服务器和网关服务器。                                                                                                          |                                                                                                                                      |

