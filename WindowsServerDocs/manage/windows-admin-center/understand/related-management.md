---
title: Windows Admin Center 相关管理解决方案
description: Windows Admin Center 如何将与进行比较和补充其他 Microsoft 监视和管理解决方案/产品 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7bf0e32b1156fe361c79ac4ccd0e3536df767e2
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296709"
---
# Windows Admin Center 和 microsoft 相关的管理解决方案

>适用于：Windows Admin Center、Windows Admin Center 预览版

[Windows Admin Center](windows-admin-center.md)是传统的框中服务器的情况下，你可能已经使用远程桌面 (RDP) 来连接到服务器以进行故障排除或配置管理工具的演进版。 它已不能用于取代其他现有的 Microsoft 管理解决方案;而是它将补充这些解决方案，如下所述。

## 远程服务器管理工具 (RSAT)

[远程服务器管理工具 (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)是 GUI 和 PowerShell 工具来管理 Windows Server 中可选角色和功能的集合。 RSAT 具有 Windows Admin Center 不具有的许多功能。 我们将来可能添加一些最常使用的工具 RSAT 中为 Windows Admin Center。 任何新的 Windows Server 角色或功能，用于管理需要 GUI 会在 Windows Admin Center 中。

## Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune)是一种基于云的企业移动性管理服务，允许你管理 iOS、 Android、 Windows 和 macOS 设备，具体取决于一组策略。 Intune 侧重于使你能够通过控制员工如何访问和共享信息保护公司信息。 相反，Windows Admin Center 不由策略驱动，但支持的 Windows 10 和 Windows Server 系统的特殊管理通过 WinRM 使用远程 PowerShell 和 WMI。

## Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/)是混合云平台，可从你的数据中心提供 Azure 服务。 使用 PowerShell 或管理员门户中，这类似于传统的 Azure 门户用于访问和管理传统的 Azure 服务管理 azure Stack。 目的不是 Windows Admin Center 管理 Azure Stack 基础结构，但你可以使用它来[管理 Azure IaaS 虚拟机](../azure/manage-azure-vms.md)（运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012） 或疑难解答单个物理Azure Stack 环境中部署的服务器。

## System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center)是本地数据中心管理解决方案部署、 配置、 管理、 监视整个数据中心。 System Center 可使你查看你的环境中的所有系统的状态，虽然 Windows Admin Center 使你可以深入了解特定服务器以管理或解决通过更精细的工具。

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **全面重新设计"框中"平台 & 工具** | **数据中心管理 & 监视** |
| 包含 Windows Server 许可证 –**产生额外费用**，就像 MMC 和其他传统的内置工具 | **全面的环境和平台的其他值的解决方案** |
| **轻量**，基于浏览器的远程管理 Windows Server 实例的**任意位置**;RDP 的替代方法 | 管理 & 监视器**异类**系统**缩放比例**，包括 HYPER-V、 VMware 和 Linux |
|**深入**单服务器 & 单个群集深入疑难解答，配置 & 维护|预配; 的基础结构自动化和自助服务; 基础结构和工作负荷监视**广度**|
|优化的管理的**各个**2 – 4 节点**HCI**群集，集成 HYPER-V、 存储空间直通和 SDN|部署 & 管理 HYPER-V、 Windows Server 群集在**数据中心范围**从使用 SCVMM**裸机**|
|仅; **HCI 监视**群集的运行状况服务将存储历史记录。 可扩展的平台，第一个和第三方**管理工具扩展**|**可扩展** & 中 SCOM，与警报、 通知、 监视; 第三方工作负荷的**可扩展监视**的平台SQL 的历史记录|
|最简单桥**混合**;载入并使用不同的数据保护、 复制、 更新和更多的 Azure 服务|**内置**数据保护、 复制和更新 (VMM/DPM/SCCM)。 与 Log Analytics 和服务映射的混合集成|
|Windows server**亮起平台功能**： 存储迁移服务、 存储副本、 系统见解等。|**其他平台**： 在 Orchestrator/SMA 自动化。与其他 SCSM & 的集成服务管理工具|

#### 每个独立; 提供目标的值**更好地一起**使用的补充功能。
