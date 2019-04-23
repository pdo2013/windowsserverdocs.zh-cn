---
title: 为使用 Azure 灾难恢复的 RDS 设置灾难恢复
description: 了解如何使用 Azure 灾难恢复，灾难恢复的 RDS 部署
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 561a515e23d12cc3397c40fd885550e735ed4d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878168"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>为使用 Azure Site Recovery 的 RDS 设置灾难恢复

>适用于：Windows 服务器 （半年频道），Windows Server 2016

Azure Site Recovery 可用于为你的远程桌面服务部署创建灾难恢复解决方案。 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview)是一种基于 Azure 的服务，安排复制、 故障转移和恢复虚拟机提供了灾难恢复功能。 Azure Site Recovery 支持多种复制技术，可以一致地复制、 保护和无缝地故障转移虚拟机以及向私钥/公钥或托管商的云应用程序。 

使用以下信息来创建和验证灾难恢复解决方案。

## <a name="disaster-recovery-deployment-options"></a>灾难恢复部署选项

你可以在物理服务器或运行的 HYPER-V 或 VMWare 虚拟机上部署 RDS。 Azure Site Recovery 可以保护本地和虚拟部署到辅助站点或 Azure。 下表显示了不同站点到站点连接和站点到 Azure 灾难 recvoery 方案中支持 RDS 部署。

| 部署类型                          | HYPER-V 站点到站点 | 站点到 Azure 的 HYPER-V | VMWare 站点到 Azure | 物理站点到 Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| 共用虚拟桌面 （非托管）       |是|否|否|否 |
| 共用虚拟桌面 （托管，没有 UPD） | 是|否|否|否|
| Remoteapp 和桌面会话 (没有 UPD) | 是|是|是|是  |

## <a name="prerequisites"></a>先决条件

可以为你的部署配置 Azure Site Recovery 之前，请确保满足以下要求：

- 创建[内部部署 RDS 部署](rds-deploy-infrastructure.md)。
- 添加[Azure 站点恢复服务保管库](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault)到 Microsoft Azure 订阅。
- 如果要将 Azure 用作恢复站点，运行[Azure 虚拟机就绪评估工具](https://azure.microsoft.com/downloads/vm-readiness-assessment/)以确保它们与 Azure Vm 和 Azure Site Recovery 服务兼容的 Vm 上。
 
## <a name="implementation-checklist"></a>实现清单

我们将介绍各种步骤来启用 Azure Site Recovery 服务为 RDS 部署中更多详细信息，但以下是高级实现步骤。

| **步骤 1-将 Vm 配置灾难恢复**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-v-下载 Microsoft Azure Site Recovery 提供程序。 在 VMM 服务器或 HYPER-V 主机上安装它。 请参阅[复制到 Azure 的使用 Azure Site Recovery 先决条件](/azure/site-recovery/site-recovery-prereq)有关信息。                                                                                                                             |
| VMWare 的配置保护服务器、 配置服务器和主目标服务器                                                                                                                                                      |
| **步骤 2-准备您的资源**                                                                                                                                                                                                           |
| 添加[Azure 存储帐户](/azure/storage/storage-create-storage-account)。                                                                                                                                                                                                              |
| HYPER-V-下载 Microsoft Azure 恢复服务代理和 HYPER-V 主机服务器上安装它。                                                                                                                                     |
| VMWare-请确保所有 Vm 上安装移动服务。                                                                                                                                                                           |
| [为 VMM 云、 HYPER-V 站点或 VMWare 站点中的 Vm 启用保护](rds-enable-dr-with-asr.md)。                                                                                                                                                                    |
| **步骤 3-设计您的恢复计划。**                                                                                                                                                                                                        |
| 映射资源-映射到 Azure Vnet 的本地网络。                                                                                                                                                                              |
| [创建恢复计划](rds-disaster-recovery-plan.md)。 |
| 通过创建测试故障转移测试恢复计划。 确保所有 Vm 可以都访问所需的资源，如 Active Directory。 确保的网络配置重定向和 rds.的工作 测试恢复计划的详细步骤，请参阅[运行测试故障转移](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **步骤 4-运行灾难恢复演练。**                                                                                                                                                                                                     |
| 运行灾难恢复演练使用计划内和计划外故障转移。 请确保所有 Vm 都有权访问所需的资源，如 Active Directory。 请确保所有 Vm 都有权访问所需的资源，如 Active Directory。 故障转移以及如何运行钻取的详细步骤，请参阅[Site Recovery 中的故障转移](/azure/site-recovery/site-recovery-failover)。|


