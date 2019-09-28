---
title: 针对 MultiPoint 服务迁移规划工作表
description: 提供了规划工作表来帮助您迁移到 Windows Server 2016 中的 MultiPoint 服务
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: d3d2ecca4062d28d210196d9191e08710eb731c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394628"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>针对 MultiPoint 服务迁移规划工作表

>适用于：Windows Server 2016

使用以下列表和表来收集 MultiPoint 服务迁移过程中所需的设置。

## <a name="source-server-settings"></a>源服务器设置

可以在 MultiPoint 管理器的 "**主页**" 选项卡上找到服务器设置。 在源服务器上使用的每个设置旁边放置一个选中标记。

- 允许一个帐户具有多个会话。
- 允许远程管理此计算机。
- 允许监视此计算机的桌面。
- 始终在控制台模式下启动。
- 第一次用户登录时不显示隐私通知。
- 为每个工作站分配一个唯一的 IP。
- 允许在此计算机上的 MultiPoint 仪表板和用户会话之间进行即时消息。
- 允许管理员和 MultiPoint 仪表板用户会话的业务流程。
- 允许工作站使用 GPU 硬件呈现。

## <a name="managed-servers-and-computers"></a>托管服务器和计算机

记录托管服务器和计算机的名称。 可以在 MultiPoint 管理器的 "**主页**" 选项卡上找到此信息。

| 计算机 | 计算机名称 |
|----------|---------------|
| 1        |               |
| 2        |               |
| 3        |               |
| 4        |               |
| 5        |               |
| 6        |               |
| 7        |               |
| 8        |               |
| 9        |               |
| 10       |               |


## <a name="stations"></a>结束

记录本地工作站及其设置。 可以在 MultiPoint 管理器的 "**工作站**" 选项卡上找到此信息。

| #  | 工作站名称 | 自动登录用户帐户 | 显示方向 |
|----|--------------|-------------------------|---------------------|
| 1  |              |                         |                     |
| 2  |              |                         |                     |
| 3  |              |                         |                     |
| 4  |              |                         |                     |
| 5  |              |                         |                     |
| 6  |              |                         |                     |
| 7  |              |                         |                     |
| 8  |              |                         |                     |
| 9  |              |                         |                     |
| 10 |              |                         |                     |

## <a name="administrators-and-multipoint-dashboard-users"></a>管理员和 MultiPoint 仪表板用户

复制管理员和 MultiPoint 仪表板用户的用户名。 可以在 MultiPoint 管理器的 "**用户**" 选项卡上找到此信息。

Administrators：

- 用户名：
- 用户名：
- 用户名：
- 用户名：
- 用户名：
- 用户名：

仪表板用户：

- 用户名：
- 用户名：
- 用户名：
- 用户名：
- 用户名：

## <a name="vdi-template-and-virtual-desktops"></a>VDI 模板和虚拟桌面

记录 VDI 模板信息和 MultiPoint 服务部署中虚拟机的名称。 可以在 MultiPoint 管理器的 "**虚拟机**" 选项卡中找到此信息。

**VDI 模板位置**： 

| # | 虚拟机名称      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |