---
title: 什么是 Windows Admin Center
description: 什么是 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 99f1a9a32ef69ba8322b2dba902003f8a750a4d2
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811652"
---
# <a name="what-is-windows-admin-center"></a>什么是 Windows Admin Center？

> 适用于：Windows Admin Center，Windows Admin Center 预览版

Windows Admin Center 是一个在本地部署的基于浏览器的管理工具集，让你能够管理 Windows Server，而无需依赖 Azure 或云。 利用 Windows Admin Center，你可以完全控制服务器基础结构的各个方面，对于在未连接到 Internet 的专用网络上进行管理特别有用。

Windows Admin Center 是“内部”管理工具（例如服务器管理器和 MMC）的现代演进版。 它补充 System Center-不能替代。

![](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>Windows Admin Center 如何工作？

Windows Admin Center 在 web 浏览器中运行，用于管理 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows 10 和通过的详细信息**Windows Admin Center 网关**Windows Server 或 Windows 10 上安装。 该网关通过使用远程 PowerShell 管理服务器，并通过 WinRM 管理 WMI。 此网关使用单个的轻型 .msi 程序包在 Windows Admin Center 中提供，你可以[下载](https://aka.ms/windowsadmincenter)它。

发布到 DNS 并提供对相应公司防火墙的访问权限后，Windows Admin Center 让你可以通过 Microsoft Edge 或 Google Chrome 从任何位置安全地连接和管理你的服务器。

![](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>了解 Windows Admin Center 如何改进管理环境

### <a name="familiar-functionality"></a>**熟悉的功能**

Windows Admin Center 是像 Microsoft 管理控制台 (MMC) 这样的长期知名管理平台的演进版本，它针对系统当前的构建和管理方式从头构建。 Windows Admin Center 包含许多你目前用于管理 Windows Server 和客户端的熟悉工具。

### <a name="easy-to-install-and-use"></a>**易于安装和使用**

在 Windows 10 计算机上[安装](../deploy/install.md)，在数分钟内即可开始管理，或在充当网关的 Windows 2016 服务器上安装，让你的整个组织都可以从 Web 浏览器管理计算机。

### <a name="complements-existing-solutions"></a>**补充现有的解决方案**

Windows Admin Center 适用于 System Center 和 Azure 管理和安全性等解决方案将添加到其功能来详细，单个计算机管理任务。

### <a name="manage-from-anywhere"></a>**从任意位置管理**

将 Windows Admin Center 网关服务器发布到公共 Internet，然后你便可以从任何位置连接并管理服务器，全部以安全的方式。

### <a name="enhanced-security-for-your-management-platform"></a>**你管理的平台的增强的安全性**

Windows Admin Center 进行了许多改进，让你的管理平台[更安全](../plan/user-access-options.md)。 基于角色的访问控制允许你微调哪些管理员有权访问哪些管理功能。 网关身份验证选项包括本地组、本地基于域的 Active Directory 和基于云的 Azure Active Directory。  此外，[还可了解](../use/logging.md)在你的环境中执行的管理操作。

### <a name="azure-integration"></a>**Azure 集成**

Windows Admin Center 有许多点[与 Azure 服务集成](../plan/azure-integration-options.md)，包括 Azure Active Directory、 Azure 备份和 Azure Site Recovery 的详细信息。

### <a name="manage-hyper-converged-clusters"></a>**管理超聚合群集**

Windows Admin Center 提供[管理超融合群集](../use/manage-hyper-converged.md)的最佳体验 - 包括虚拟化计算、存储和网络组件。

### <a name="extensibility"></a>**可扩展性**

Windows Admin Center 在构建之初就考虑了可扩展性，它为 Microsoft 和第三方开发人员提供了除当前产品和服务外构建其他工具和解决方案的能力。 Microsoft 提供允许开发人员构建自己的 Windows Admin Center 工具的 [SDK](../extend/extensibility-overview.md) 。

> [!Tip]
> 已准备好安装 Windows Admin Center？ [立即下载](https://aka.ms/windowsadmincenter)
