---
title: 使用 Azure 灾难恢复为 RDS 设置灾难恢复
description: 了解如何使用 Azure 灾难恢复对 RDS 部署实现灾难恢复
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 514262fde3b433baf89fe8f5a0cf8b04ef267354
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387545"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>使用 Azure Site Recovery 为 RDS 设置灾难恢复

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

可以使用 Azure Site Recovery 为远程桌面服务部署创建灾难恢复解决方案。 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) 是一种基于 Azure 的服务，通过安排虚拟机的复制、故障转移和恢复来提供灾难恢复功能。 Azure Site Recovery 支持多种复制技术，以便一致地复制、保护虚拟机和云应用程序，并将其无缝地故障转移到私有云/公有云或托管商的云。 

使用以下信息可创建和验证灾难恢复解决方案。

## <a name="disaster-recovery-deployment-options"></a>灾难恢复部署选项

可以在运行 Hyper-V 或 VMWare 的物理服务器或虚拟机上部署 RDS。 Azure Site Recovery 可以将本地和虚拟部署保护到辅助站点或 Azure。 下表显示站点到站点和站点到 Azure 灾难恢复方案中支持的不同 RDS 部署。

| 部署类型                          | Hyper-V 站点到站点 | Hyper-V 站点到 Azure | VMWare 站点到 Azure | 物理站点到 Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| 共用虚拟桌面（非托管）       |是|否|否|否 |
| 共用虚拟桌面（托管，无 UPD） | 是|否|否|否|
| RemoteApp 和桌面会话（无 UPD） | 是|是|是|是  |

## <a name="prerequisites"></a>必备条件

请先确保满足以下要求，然后才能为部署配置 Azure Site Recovery：

- 创建[本地 RDS 部署](rds-deploy-infrastructure.md)。
- 将 [Azure Site Recovery 服务保管库](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault)添加到 Microsoft Azure 订阅。
- 如果要将 Azure 用作恢复站点，请在 VM 上运行 [Azure 虚拟机就绪情况评估工具](https://azure.microsoft.com/downloads/vm-readiness-assessment/)以确保它们与 Azure VM 和 Azure Site Recovery 服务兼容。
 
## <a name="implementation-checklist"></a>实现清单

我们会更详细地介绍用于为 RDS 部署启用 Azure Site Recovery 服务的各种步骤，但下面是高级实现步骤。

| **步骤 1 - 配置 VM 以实现灾难恢复**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V - 下载 Microsoft Azure Site Recovery 提供程序。 在 VMM 服务器或 Hyper-V 主机上安装它。 有关信息，请参阅[使用 Azure Site Recovery 复制到 Azure 的先决条件](/azure/site-recovery/site-recovery-prereq)。                                                                                                                             |
| VMWare - 配置保护服务器、配置服务器和主目标服务器                                                                                                                                                      |
| **步骤 2 - 准备资源**                                                                                                                                                                                                           |
| 添加 [Azure 存储帐户](/azure/storage/storage-create-storage-account)。                                                                                                                                                                                                              |
| Hyper-V - 下载 Microsoft Azure 恢复服务代理并在 Hyper-V 主机服务器上安装它。                                                                                                                                     |
| VMWare - 确保在所有 VM 上安装出行服务。                                                                                                                                                                           |
| [为 VMM 云、Hyper-V 站点或 VMWare 站点中的 VM 启用保护](rds-enable-dr-with-asr.md)。                                                                                                                                                                    |
| **步骤 3 - 设计恢复计划。**                                                                                                                                                                                                        |
| 映射资源 - 将本地网络映射到 Azure VNET。                                                                                                                                                                              |
| [创建恢复计划](rds-disaster-recovery-plan.md)。 |
| 通过创建测试故障转移来测试恢复计划。 确保所有 VM 都可以访问所需资源，如 Active Directory。 确保为 RDS 配置了网络重定向并正常工作。 有关测试恢复计划的详细步骤，请参阅[运行测试故障转移](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **步骤 4 - 运行灾难恢复演练。**                                                                                                                                                                                                     |
| 使用计划内和计划外故障转移运行灾难恢复演练。 确保所有 VM 都有权访问所需资源，如 Active Directory。 确保所有 VM 都有权访问所需资源，如 Active Directory。 有关故障转移以及如何运行演练的详细步骤，请参阅 [Site Recovery 中的故障转移](/azure/site-recovery/site-recovery-failover)。|


