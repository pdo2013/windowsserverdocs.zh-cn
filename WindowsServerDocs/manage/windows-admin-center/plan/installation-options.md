---
title: 哪种类型是安装的最适合你
description: 本主题介绍 Windows Admin Center，包括由多个管理员在 Windows 10 电脑或使用的 Windows 服务器上安装的不同安装选项。
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 9b26ce28d8b3f74c26adab87e68b9985f2be5361
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811813"
---
# <a name="what-type-of-installation-is-right-for-you"></a>哪种安装类型适合你？

>适用于：Windows Admin Center，Windows Admin Center 预览版

本主题介绍 Windows Admin Center，包括由多个管理员在 Windows 10 电脑或使用的 Windows 服务器上安装的不同安装选项。 若要在 Azure 中的 VM 上安装 Windows Admin Center，请参阅[在 Azure 中部署 Windows Admin Center](../azure/deploy-wac-in-azure.md)。

## <a name="supported-operating-systems-installation"></a>支持的操作系统：安装

你可以**安装**以下 Windows 操作系统上的 Windows Admin Center:

| **版本**  | **安装模式** |
| -------------| -----------------------|
| Windows 10 版本 1709 或更高版本 | 桌面模式 |
| Windows Server 半年频道 | 网关模式 |
| Windows Server 2016 | 网关模式 |
| Windows Server 2019 | 网关模式 |

**桌面模式：** 从开始菜单启动以及从其安装同一台计算机连接到 Windows Admin Center 网关 (即`https://localhost:6516`)

**网关模式：** 从另一台计算机上的客户端浏览器连接到 Windows Admin Center 网关 (即`https://servername.contoso.com`) 

> [!WARNING]
> 不支持在域控制器上安装 Windows Admin Center。 [了解有关域控制器安全详细信息的最佳做法](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack)。 

> [!IMPORTANT]
> 无法使用 Internet Explorer 来管理 Windows Admin Center-而是需要使用[支持的浏览器](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
)。  如果在 Windows Server 上安装 Windows Admin Center，我们建议管理的远程连接使用 Windows 10 和边缘。  或者，你可以管理本地 Windows Server 上如果已安装支持的浏览器。

## <a name="supported-operating-systems-management"></a>支持的操作系统：Management

你可以**管理**以下 Windows 操作系统使用 Windows Admin Center:

| Version | 管理*节点*通过*服务器管理器* | 管理*群集*通过*故障转移群集管理器* | 管理*HCI*通过*HCI 群集管理器* |
| ------------------------- |--------------- | ----- | ------------------------ |
| Windows 10 版本 1709 或更高版本 | 是 （通过计算机管理） | 不可用 | 不可用 |
| Windows Server 半年频道 | 是 | 是 | 不可用 |
| Windows Server 2019 | 是 | 是 | 是 |
| Windows Server 2016 | 是 | 是 | 是，使用[最新累积更新](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | 是 | 是 | 不可用 |
| Windows Server 2012 R2 | 是 | 是 | 不可用 |
| Microsoft Hyper-V Server 2012 R2 | 是 | 是 | 不可用 |
| Windows Server 2012 | 是 | 是 | 不可用 |
| Windows Server 2008 R2 | 是的有限的功能 | 不可用 | 不可用 |

> [!NOTE]
> Windows Admin Center 需要的 Windows Server 2008 R2，2012年和 2012 R2 中不包括 PowerShell 功能。 如果将管理其与 Windows Admin Center，您将需要这些服务器上安装 Windows Management Framework (WMF) 5.1 或更高版本。
> 
> 在 PowerShell 中键入 `$PSVersiontable`，以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。 
> 
> 如果未安装 WMF，你可以[下载 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

## <a name="deployment-options"></a>部署选项

| ![i m g](../media/deployment-options/W10.png) | ![i m g](../media/deployment-options/gateway.png) | ![i m g](../media/deployment-options/node.png) | ![i m g](../media/deployment-options/HA.png) |
| --------------------------------------------- | ------------------------------------------------- |----------------------------------------------|-------------------------------------------- |
|                                             |                                                 |                                              |                                            |

| 本地客户端 | 网关服务器 | 托管服务器 | 故障转移群集 |
| --- | --- | --- | --- |
| 在已连接到托管服务器的本地 Windows 10 客户端上安装。  适用于快速入门中，测试、 临时或小范围方案。 |指定的网关服务器上安装并从任何客户端浏览器访问连接到网关服务器。  适用于大规模部署方案。 | 直接在托管的服务器，以管理本身或它所在的成员节点的群集上安装。  适用于分布式方案。 | 若要启用网关服务的高可用性的故障转移群集中部署。 适用于生产环境，以确保你的管理服务的复原能力。 |

## <a name="high-availability"></a>高可用性

可以通过在故障转移群集上的主动-被动模型中部署 Windows Admin Center 启用网关服务的高可用性。 如果一个群集中节点出现故障，Windows Admin Center 正常故障转移到另一个节点，让你继续无缝地管理您的环境中的服务器。

[了解如何将 Windows Admin Center 部署具有高可用性。](../deploy/high-availability.md)

> [!Tip]
> 已准备好安装 Windows Admin Center？ [立即下载](https://aka.ms/windowsadmincenter)
