---
title: 使用 Azure Site Recovery 启用 RDS 的灾难恢复
description: 了解如何使用 Azure Site Recovery 启用 RDS 的灾难恢复。
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
ms.openlocfilehash: 7aa25602c71e5d114be7ae59c5e3ce168844d700
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446556"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>使用 Azure Site Recovery 启用 RDS 的灾难恢复

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

若要确保充分配置 RDS 部署进行灾难恢复，需要保护构成 RDS 部署的所有组件：

- Active Directory
- SQL Server 层
- RDS 组件
- 网络组件

## <a name="configure-active-directory-and-dns-replication"></a>配置 Active Directory 和 DNS 复制

在灾难恢复站点上需要 Active Directory，才能使 RDS 部署生效。 基于 RDS 部署的复杂程度，有以下两个选项：

- 选项 1 - 如果整个本地站点有少量应用程序和单个域控制器，并且需要故障转移整个站点，请使用 ASR 复制将域控制器复制到辅助站点（适用于站点到站点和站点到 Azure 方案）。
- 选项 2 - 如果有大量应用程序，正在运行 Active Directory 林，并且需要一次故障转移多个应用程序，请在灾难恢复站点上设置其他域控制器（辅助站点或 Azure 中）。

有关使域控制器在灾难恢复站点上可用的详细信息，请参阅[使用 Azure Site Recovery 保护 Active Directory 和 DNS](/azure/site-recovery/site-recovery-active-directory)。 对于本指南的其余部分，我们假定你已执行这些步骤并使域控制器可用。

## <a name="set-up-sql-server-replication"></a>设置 SQL Server 复制

有关设置 SQL Server 复制的步骤，请参阅[使用 SQL Server 灾难恢复和 Azure Site Recovery 保护 SQL Server](/azure/site-recovery/site-recovery-sql)。

## <a name="enable-protection-for-the-rds-application-components"></a>为 RDS 应用程序组件启用保护

根据你的 RDS 部署类型，可以在 Azure Site Recovery 中为不同的组件 VM（如下表中列出）启用保护。 根据是在 Hyper-V 上还是在 VMWare 上部署 VM，配置相关的 Azure Site Recovery 元素。


|               部署类型                |                                                                                                     保护步骤                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     个人虚拟桌面（非托管）     | 1.确保所有虚拟化主机均已安装有 RDVH 角色。    </br>2.连接代理。  </br>3.个人台式机。 </br>4.黄金模板 VM。 </br>5.Web 访问、许可证服务器和网关服务器 |
| 池虚拟桌面（托管，无 UPD） |                    1.所有虚拟化主机均已安装有 RDVH 角色。  </br>2.连接代理。  </br>3.黄金模板 VM。 </br>4.Web 访问、许可证服务器和网关服务器。                    |
|   RemoteApp 和桌面会话（无 UPD）   |                                                          1.会话主机。  </br>2.连接代理。 </br>3.Web 访问、许可证服务器和网关服务器。                                                           |

