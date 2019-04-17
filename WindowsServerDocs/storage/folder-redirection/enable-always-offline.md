---
title: 启用始终脱机模式更快地访问文件
description: 如何使用脱机文件的始终脱机模式提供更快地访问缓存的文件和重定向的文件夹。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc54b1e33d09e7f2b9eea01e4f09fb83f13dc1af
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831387"
---
# 启用始终脱机模式更快地访问文件

>适用于： Windows 10，Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2，Windows Server 2016

本文档介绍了如何使用脱机文件的始终脱机模式提供更快地访问缓存的文件和重定向的文件夹。 始终脱机还提供了低带宽使用量，因为用户始终脱机工作，即使它们连接时通过高速网络连接。

## 必备条件

若要启用始终脱机模式，你的环境必须满足以下先决条件。

- 带有客户端计算机加入域的 Active Directory 域服务 (AD DS) 域。 没有林或域功能级别要求或架构要求。
- 运行 Windows 10、 Windows 8.1、 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的客户端计算机。 （运行早期版本的 Windows 客户端计算机可能会继续转换到高速网络连接的联机模式。）
- 与安装的组策略管理计算机。

## 启用始终脱机模式

若要启用始终脱机模式，使用组策略启用**配置慢速链接模式**策略设置，并将延迟设置为**1** （毫秒）。 这样做会导致客户端计算机运行 Windows 8 或 Windows Server 2012 自动使用始终脱机模式。

>[!NOTE]
>运行 Windows 7 计算机，Windows Vista、 Windows Server 2008 R2 或 Windows Server 2008 可能会继续转换到联机模式如果网络连接的延迟低于 1 毫秒。

1. 打开**组策略管理**。
2. 若要选择创建新组策略对象 (GPO) 的脱机文件设置，右键单击相应的域或组织单位 (OU)，然后选择**创建此域中的 GPO 并在此处链接**。
3. 在控制台树中，右键单击你想要配置的脱机文件设置，然后选择**编辑**的 GPO。 将出现**组策略管理编辑器**。
4. 在控制台树中，在**计算机配置**下展开**策略**、**管理模板**、**网络**，然后展开**脱机文件**。
5. **配置慢速链接模式**中，右键单击，然后选择**编辑**。 **配置慢速链接模式**窗口将显示。
6. 选中**已启用**。
7. 在**选项**框中，选择**显示**。 **显示内容窗口**将显示。
8. 在**值名称**框中，指定你想要启用始终脱机模式的文件共享。
9. 若要启用始终脱机模式下并在所有的文件共享，请输入**\***。
10. 在**值**框中，输入**延迟 = 1**以将延迟阈值设置为 1 毫秒，然后选择**确定**。

>[!NOTE]
>默认情况下，在始终脱机模式下，Windows 同步文件中脱机文件缓存在后台每隔两个小时。 若要更改此值，使用**配置后台同步**策略设置。

## 详细信息

* [文件夹重定向、 脱机文件和漫游用户配置文件概述](folder-redirection-rup-overview.md)
* [部署使用脱机文件的文件夹重定向](deploy-folder-redirection.md)