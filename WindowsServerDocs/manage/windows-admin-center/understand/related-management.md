---
title: Windows Admin Center 相关的管理解决方案
description: Windows Admin Center 与其他 Microsoft 监视和管理解决方案/产品 (Project Honolulu) 的比较及其如何相辅相成
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7bf0e32b1156fe361c79ac4ccd0e3536df767e2
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63742127"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center 和相关的 Microsoft 管理解决方案

>适用于：Windows Admin Center、Windows Admin Center 预览版

[Windows Admin Center](windows-admin-center.md) 是传统的内置服务器管理工具的演进。这些传统的方法包括使用远程桌面 (RDP) 连接到服务器进行故障排除或配置。 Windows Admin Center 并不旨在取代其他现有的 Microsoft 管理解决方案，而是对这些解决方案进行补充，如下所述。

## <a name="remote-server-administration-tools-rsat"></a>远程服务器管理工具 (RSAT)

[远程服务器管理工具 (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) 是用于管理 Windows Server 中可选角色和功能的 GUI 与 PowerShell 工具集合。 RSAT 具有 Windows Admin Center 所不能提供的许多功能。 将来，我们可能会将 RSAT 中的某些最常用工具添加到 Windows Admin Center。 需要使用 GUI 进行管理的任何新 Windows Server 角色或功能将在 Windows Admin Center 中提供。

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) 是一个基于云的企业移动性管理服务，可让你根据一组策略管理 iOS、Android、Windows 和 macOS 设备。 Intune 重点用于保护公司信息，它可以控制员工访问和共享信息的方式。 相反，Windows Admin Center 不是由策略驱动，而是使用远程 PowerShell 和基于 WinRM 的 WMI 实现 Windows 10 与 Windows Server 系统的即席管理。

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) 是一个混合云平台，可用于从数据中心交付 Azure 服务。 可以使用 PowerShell 或管理员门户管理 Azure Stack，该门户类似于用于访问和管理传统 Azure 服务的传统 Azure 门户。 Windows Admin Center 不适合用于管理 Azure Stack 基础结构，但你可以使用它来[管理 Azure IaaS 虚拟机](../azure/manage-azure-vms.md)（运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012），或者对 Azure Stack 环境中部署的各个物理服务器进行故障排除。

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) 是用于部署、配置、管理和监视整个数据中心的本地数据中心管理解决方案。 使用 System Center 可以查看环境中所有系统的状态，而使用 Windows Admin Center 可以深入到特定的服务器，以使用更精细的工具对其进行管理或故障排除。

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **重新构想了“内置”平台和工具** | **数据中心管理和监视** |
| 与 MMC 和其他传统内置工具一样**免费**随附 Windows Server 许可证 | 可为整个环境和平台带来更大价值的**综合性**解决方案套件 |
| 基于浏览器的**轻型**解决方案，可在**任意位置**对 Windows Server 实例进行远程管理；可以替代 RDP | **大规模**管理和监视**异构**系统，包括 Hyper-V、VMware 和 Linux |
|**深度**下钻到单个服务器和单个群集，以进行故障排除、配置和维护|基础结构预配；自动化和自助服务；基础结构和工作负荷监视**广度**|
|**单个** 2-4 节点 **HCI** 群集的优化管理、集成 Hyper-V、存储空间直通和 SDN|使用 SCVMM 以**数据中心的规模**从**祼机**部署和管理 Hyper-V、Windows Server 群集|
|仅限**在 HCI 上进行监视**；群集运行状况服务可存储历史记录。 第一方和第三方**管理员工具扩展**的可扩展平台|SCOM 中的**可扩展**、**可缩放监视**平台，提供警报、通知和第三方工作负荷监视；SQL 提供历史记录|
|最简单的**混合**环境桥梁；载入并使用各种 Azure 服务进行数据保护、复制、更新等|**内置**数据保护、复制和更新 (DPM/VMM/SCCM)。 与 Log Analytics 和服务映射的混合集成|
|Windows Server 的**平台功能亮点**：存储迁移服务、存储副本、系统见解，等等。|**其他平台**：业务流程协调程序/SMA 自动化。与 SCSM 和其他服务管理工具相集成|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>每个平台都可独立提供有针对性的价值；与互补功能**结合使用效果更佳**。
