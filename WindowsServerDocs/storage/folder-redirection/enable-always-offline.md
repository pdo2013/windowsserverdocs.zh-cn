---
title: 启用更快地访问文件始终脱机模式
description: 如何使用脱机文件的始终脱机模式提供更快地访问缓存的文件和重定向的文件夹。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: ddf6a816e417c2eddff090df8dba841a894a3255
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447672"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>启用更快地访问文件始终脱机模式

>适用于：Windows 10、 Windows 8、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012、 Windows Server 2012 R2 和 Windows （半年频道）

本文档介绍如何使用脱机文件的始终脱机模式提供更快地访问缓存的文件和重定向的文件夹。 始终脱机还提供了较低的带宽使用率，因为用户始终脱机工作，即使它们连接时通过高速网络连接。

## <a name="prerequisites"></a>先决条件

若要启用始终脱机模式下，你的环境必须满足以下先决条件。

- 具有客户端计算机加入到域的 Active Directory 域服务 (AD DS) 域。 没有林或域功能级别要求或架构要求。
- 运行 Windows 10、 Windows 8.1，Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的客户端计算机。 （运行早期版本的 Windows 客户端计算机可能会持续转换到联机模式非常高速网络连接上。）
- 使用安装组策略管理的计算机。

## <a name="enable-always-offline-mode"></a>启用始终脱机模式

若要启用始终脱机模式下，使用组策略启用**配置慢速链接模式**策略设置并将延迟时间设置为**1** （毫秒）。 这样做会导致客户端计算机运行 Windows 8 或 Windows Server 2012 自动使用始终脱机模式。

>[!NOTE]
>运行 Windows 7 的计算机，Windows Vista、 Windows Server 2008 R2 或 Windows Server 2008 可能会继续转换到联机模式下如果网络连接的延迟降至低于 1 毫秒。

1. 打开**组策略管理**。
2. 要根据需要创建脱机文件设置为新组策略对象 (GPO)，请右键单击相应的域或组织单位 (OU)，然后选择**在此域中创建 GPO 并在此处链接**。
3. 在控制台树中，右键单击您要为其配置脱机文件设置，然后选择的 GPO**编辑**。 **组策略管理编辑器**出现。
4. 在控制台树中下,**计算机配置**，展开**策略**，展开**管理模板**，展开**网络**，展开**脱机文件**。
5. 右键单击**配置慢速链接模式**，然后选择**编辑**。 **配置慢速链接模式**窗口中会显示。
6. 选择“已启用”  。
7. 在中**选项**框中，选择**显示**。 **显示内容窗口**将出现。
8. 在中**值名称**框中，指定你想要启用始终脱机模式下的文件共享。
9. 若要启用始终脱机模式下的所有文件共享上，输入 * *\\* * *。
10. 在中**值**框中，输入**延迟 = 1**以将延迟阈值设置为一毫秒，然后选择**确定**。

>[!NOTE]
>默认情况下，在始终脱机模式下，Windows 将文件同步脱机文件缓存在后台中每隔两小时。 若要更改此值，请使用**配置背景同步**策略设置。

## <a name="more-information"></a>详细信息

* [文件夹重定向、 脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
* [部署使用脱机文件的文件夹重定向](deploy-folder-redirection.md)