---
title: Windows Admin Center 相关的管理解决方案
description: 如何 Windows Admin Center 将与进行比较，并补充了其他 Microsoft 监视和管理解决方案/产品 (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 385a066cb828f58d698c2ca47e0553e996a77733
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847318"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center 和 microsoft 相关的管理解决方案

>适用于：Windows Admin Center，Windows Admin Center 预览版

[Windows Admin Center](windows-admin-center.md)是传统的框中服务器的发展的情况下，你可能已使用远程桌面 (RDP) 连接到服务器进行故障排除或配置管理工具。 它具有不旨在取代其他现有的 Microsoft 管理解决方案;而是它补充这些解决方案，如下所述。

## <a name="remote-server-administration-tools-rsat"></a>远程服务器管理工具 (RSAT)

[远程服务器管理工具 (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)是 GUI 和 PowerShell 工具来管理 Windows Server 中可选角色和功能的集合。 RSAT 有 Windows Admin Center 不具有的许多功能。 我们可能会添加一些最常用的工具 RSAT 中到 Windows Admin Center 将来。 任何新的 Windows Server 角色或功能需要 GUI 的管理将在 Windows Admin Center。

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune)是一种基于云的企业移动性管理服务，你可以管理 iOS、 Android、 Windows 和 macOS 设备，基于一组策略。 Intune 重点介绍您可以通过控制员工访问和共享信息的方式来保护公司信息。 与此相反，Windows Admin Center 不是策略驱动的但启用即席管理的 Windows 10 和 Windows Server 系统，使用通过 WinRM 的远程 PowerShell 和 WMI。

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/)是一种混合云平台，让你从你的数据中心提供 Azure 服务。 使用 PowerShell 或管理员门户中，这类似于传统的 Azure 门户用于访问和管理传统的 Azure 服务管理 azure Stack。 Windows Admin Center 并不旨在管理 Azure Stack 基础结构，但可以使用它到[管理 Azure IaaS 虚拟机](../configure/manage-azure-vms.md)（运行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012） 或进行故障排除在 Azure Stack 环境中部署的各个物理服务器。

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center)是在本地数据中心管理解决方案进行部署、 配置、 管理、 监视整个数据中心。 System Center，可以看到你的环境中的所有系统的状态，而 Windows Admin Center 允许您向下钻取到特定的服务器来管理或使用更精细的工具进行故障排除。

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **重新构想的"现成"平台和工具** | **数据中心管理和监视** |
| 附带的 Windows Server 许可证 –**无需额外付费**，就像 MMC 和其他传统的现成工具 | **全面**一套跨环境和平台的更多价值的解决方案 |
| **轻型**，基于浏览器的 Windows Server 实例的远程管理**任意位置**; 替代通过 rdp 连接 | 管理和监视**异类**系统**大规模**，包括 HYPER-V、 VMware 和 Linux |
|**深度**单服务器和单个群集的向下钻取有关故障排除，配置和维护|基础结构预配;自动化和自助服务; 基础结构和工作负荷监视**广度**|
|优化的管理**各个**2 – 4 节点**HCI**群集，集成的 HYPER-V、 存储空间直通和 SDN|部署和管理的 HYPER-V、 Windows Server 群集上**数据中心规模**从**祼机**与 SCVMM|
|**监视 HCI** ; 仅群集运行状况服务存储历史记录。 为第 1 个和第三方的可扩展平台**管理员工具扩展**|**可扩展** & **可扩展监视**平台在 SCOM 中，使用警报、 通知、 第三方工作负荷监视;SQL 的历史记录|
|最简单到桥**混合**; 载入并使用各种 Azure 服务进行数据保护、 复制、 更新和的详细信息|**内置**数据保护、 复制和更新 (DPM/VMM/SCCM)。 与 Log Analytics 和服务映射的混合集成|
|**平台功能点亮**的 Windows Server:存储迁移服务，存储副本，系统见解，等等。|**其他平台**:业务流程协调程序/SMA 中的自动化。使用 SCSM 和其他服务管理工具集成|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>每个独立; 提供目标的值**更好地一起**互为补充的功能。
