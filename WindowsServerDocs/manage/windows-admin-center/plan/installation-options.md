---
title: 哪种安装类型适合你
description: 本主题介绍 Windows Admin Center，包括由多个管理员安装 Windows 10 电脑或使用 Windows server 上的不同的安装选项。
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: cc6f9e6c2f2572618c1c5fdae00047d24fbeb3cf
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296659"
---
# 哪种安装类型适合你？

>适用于：Windows Admin Center、Windows Admin Center 预览版

本主题介绍 Windows Admin Center，包括由多个管理员安装 Windows 10 电脑或使用 Windows server 上的不同的安装选项。 若要在 Azure 中的 VM 上安装 Windows Admin Center，请参阅[在 Azure 中部署 Windows Admin Center](../azure/deploy-wac-in-azure.md)。

## 支持的操作系统： 安装

你可以**安装**Windows Admin Center 在以下 Windows 操作系统上：

| **版本** | **安装模式** |
|-------------|-----------------------|
|Windows 10 版本 1709 或更高版本 | 桌面模式 |
|Windows Server 半年频道 | 网关模式 |
|Windows Server 2016 | 网关模式 |
|Windows Server 2019 | 网关模式 |

**桌面模式：** 从开始菜单启动并连接到 Windows Admin Center 网关从安装它的同一计算机 (即`https://localhost:6516`)

**网关模式：** 从另一台计算机上的客户端浏览器连接到 Windows Admin Center 网关 (即`https://servername.contoso.com`) 

> [!WARNING]
> 不支持在域控制器上安装 Windows Admin Center。 [阅读有关域控制器安全最佳做法的详细信息](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack)。 

> [!IMPORTANT]
> 你不能使用 Internet Explorer 管理 Windows Admin Center-必须改为使用[支持浏览器](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
)。  如果你 Windows Server 上安装 Windows Admin Center，我们建议通过使用 Windows 10 和 Edge 连接远程管理。  或者，你可以管理本地 Windows Server 上如果已安装的受支持的浏览器。

## 支持的操作系统： 管理

你可以**管理**以下 Windows 操作系统使用 Windows Admin Center:

| 版本 | 管理通过*服务器管理器**节点* | 管理*群集*通过*故障转移群集管理器* | 管理*HCI*通过*HCI 群集管理器*|
|-------------------------|---------------|-----|------------------------|
| Windows 10 版本 1709 或更高版本 | 是 （通过计算机管理） | 不适用 | 不适用 |
| Windows Server 半年频道 | 是 | 是 | 不适用 |
| Windows Server 2019 | 是 | 是 | 是 |
| Windows Server 2016 | 是 | 是 | 是，带有[最新累积更新](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | 是 | 是 | 不适用 |
| Windows Server 2012 R2 | 是 | 是 | 不适用 |
| Microsoft Hyper-V Server 2012 R2 | 是 | 是 | 不适用 |
| Windows Server 2012 | 是 | 是 | 不适用 |
| Windows Server 2008 R2 | 是，有限的功能 | 不适用 | 不适用 |

> [!NOTE]
> Windows Admin Center 需要的 PowerShell 功能不包括在 Windows Server 2008 R2、 2012年和 2012 R2 中。 如果你将能够管理这些使用 Windows Admin Center，你将需要在这些服务器上安装 Windows Management Framework (WMF) 5.1 或更高版本。

>在 PowerShell 中键入 `$PSVersiontable` 以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。 

>如果未安装 WMF，你可以[下载 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

## 部署选项

| ![img](../media/deployment-options/W10.png) | ![img](../media/deployment-options/gateway.png) | ![img](../media/deployment-options/node.png) | ![img](../media/deployment-options/HA.png) |
|---|---|---|---|

| 本地客户端 | 网关服务器 | 托管的服务器 | 故障转移群集 |
| --- | --- | --- | --- |
| 在本地 Windows 10 客户端连接到托管服务器上安装。  适合快速入门，测试、 临时或小范围的方案。 |在指定的网关服务器上安装和使用从访问任何客户端浏览器连接到网关服务器。  适合大规模的方案。 | 在托管的服务器，以管理本身或它在成员节点在群集上直接安装。  适合分布式方案。 | 在故障转移群集以实现高可用性的网关服务中部署。 适合用于生产环境来确保你管理服务的复原能力。 |

## 高可用性

你可以通过在故障转移群集上的主动被动模型中部署 Windows Admin Center 启用网关服务的高可用性。 如果其中一个群集中节点出现故障，Windows Admin Center 顺利故障转移到另一个节点，从而使你继续无缝地管理你的环境中的服务器。

[了解如何使用高可用性部署 Windows Admin Center。](../deploy/high-availability.md)

> [!Tip]
> 已准备好安装 Windows Admin Center？ [立即下载](https://aka.ms/windowsadmincenter)
