---
title: 适合你的安装类型
description: 本主题介绍 Windows 管理中心的不同安装选项, 包括在 Windows 10 电脑或 Windows server 上安装以供多个管理员使用。
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 36c9dfcb38ef417df56206cdb18633cc877183c4
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658898"
---
# <a name="what-type-of-installation-is-right-for-you"></a>哪种安装类型适合你？

>适用于：Windows Admin Center、Windows Admin Center 预览版

本主题介绍 Windows 管理中心的不同安装选项, 包括在 Windows 10 电脑或 Windows server 上安装以供多个管理员使用。 若要在 Azure 中的 VM 上安装 Windows 管理中心, 请参阅[在 azure 中部署 Windows 管理中心](../azure/deploy-wac-in-azure.md)。

## <a name="installation-types"></a>安装：类型

| 本地客户端                                | 网关服务器                                  | 托管服务器                               | 故障转移群集                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| ![i m g](../media/deployment-options/W10.PNG) | ![i m g](../media/deployment-options/gateway.PNG) | ![i m g](../media/deployment-options/node.PNG) | ![i m g](../media/deployment-options/HA.png) |
| 在连接到托管服务器的本地 Windows 10 客户端上安装。  适用于快速入门、测试、即席或小规模方案。 |在指定的网关服务器上安装并从连接到网关服务器的任何客户端浏览器访问。  适用于大规模方案。 | 直接安装在托管服务器上, 以管理自身或其成员节点所在的群集。  对于分布式方案非常有用。 | 在故障转移群集中部署以启用网关服务的高可用性。 适用于生产环境, 以确保管理服务的复原能力。 |

## <a name="installation-supported-operating-systems"></a>安装：受支持的操作系统

可以在以下 Windows 操作系统上**安装**windows 管理中心:

| **平台**                       | **安装模式** |
| -----------------------------------| --------------------- |
| Windows 10 版本1709或更高版本  | 本地客户端 |
| Windows Server 半年频道 | 网关服务器, 托管服务器, 故障转移群集 |
| Windows Server 2016                | 网关服务器, 托管服务器, 故障转移群集 |
| Windows Server 2019                | 网关服务器, 托管服务器, 故障转移群集 |

操作 Windows 管理中心:

- **在本地客户端方案中:** 从 "开始" 菜单启动 Windows 管理中心网关, 并通过访问`https://localhost:6516`, 从客户端 web 浏览器连接到该网关。
- **在其他方案中:** 通过客户端浏览器从客户端浏览器连接到另一台计算机上的 Windows 管理中心网关, 例如,`https://servername.contoso.com`

> [!WARNING]
> 不支持在域控制器上安装 Windows 管理中心。 [阅读有关域控制器安全最佳做法的详细信息](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack)。 

## <a name="installation-supported-web-browsers"></a>安装：支持的 web 浏览器

Microsoft Edge 和 Google Chrome 在 Windows 10 上经过测试和支持。 其他 web 浏览器 (包括 Internet Explorer 和 Firefox) 当前不是我们的测试矩阵的一部分, 因此不受*正式*支持。 这些浏览器在运行 Windows 管理中心时可能会遇到问题。 例如, Firefox 具有自己的证书存储区, 因此必须将该`Windows Admin Center Client`证书导入到 Firefox, 才能在 windows 10 上使用 windows 管理中心。 有关更多详细信息, 请参阅[特定于浏览器的已知问题](../support/known-issues.md#browser-specific-issues)。

## <a name="management-target-supported-operating-systems"></a>管理目标:受支持的操作系统

你可以使用 Windows 管理中心**管理**以下 windows 操作系统:

| Version | 通过*服务器管理器*管理*节点* | 通过*故障转移群集管理器*管理*群集* | 通过*Hci 群集管理器*管理*hci* |
| ------------------------- |--------------- | ----- | ------------------------ |
| Windows 10 版本1709或更高版本 | 是 (通过计算机管理) | 不可用 | 不可用 |
| Windows Server 半年频道 | 是 | 是 | 不可用 |
| Windows Server 2019 | 是 | 是 | 是 |
| Windows Server 2016 | 是 | 是 | 是, 具有[最新累积更新](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | 是 | 是 | 不可用 |
| Windows Server 2012 R2 | 是 | 是 | 不可用 |
| Microsoft Hyper-V Server 2012 R2 | 是 | 是 | 不可用 |
| Windows Server 2012 | 是 | 是 | 不可用 |
| Windows Server 2008 R2 | 是, 有限的功能 | 不可用 | 不可用 |

> [!NOTE]
> Windows 管理中心需要未包含在 Windows Server 2008 R2、2012和 2012 R2 中的 PowerShell 功能。 如果你将通过 Windows 管理中心管理这些这些服务器, 你将需要在这些服务器上安装 Windows Management Framework (WMF) 版本5.1 或更高版本。
> 
> 在 PowerShell 中键入 `$PSVersiontable`，以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。 
> 
> 如果未安装 WMF, 你可以[下载 wmf 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

## <a name="high-availability"></a>高可用性

可以通过在故障转移群集上的主动-被动模式中部署 Windows 管理中心来启用网关服务的高可用性。 如果群集中的一个节点发生故障, Windows 管理中心中心会正常故障转移到另一个节点, 从而使你可以在环境中无缝地继续管理服务器。

[了解如何部署具有高可用性的 Windows 管理中心。](../deploy/high-availability.md)

> [!Tip]
> 已准备好安装 Windows Admin Center？ [立即下载](https://aka.ms/windowsadmincenter)
