---
title: MultiPoint 服务迁移的规划工作表
description: 提供了规划工作表来帮助您迁移到 Windows Server 2016 中的 MultiPoint 服务
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: a9d9b62bced9be90c658b79338c6f4ef07710fc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880578"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>MultiPoint 服务迁移的规划工作表

>适用于：Windows Server 2016

使用下面的列表和表来收集在 MultiPoint 服务迁移过程所需的设置。

## <a name="source-server-settings"></a>源服务器设置

您可以在找到服务器设置**主页**MultiPoint 管理器中的选项卡。 将每个设置旁边的选中标记放置在源服务器上使用。

- 允许一个帐户进行了多个会话。
- 允许远程管理此计算机。
- 允许此计算机的桌面的监视。
- 始终在控制台模式下启动。
- 在第一个用户登录时不显示隐私通知。
- 为每个工作站分配一个唯一的 IP。
- 在此计算机上允许 MultiPoint 仪表板和用户会话之间的即时消息。
- 允许管理员和 MultiPoint 仪表板用户会话的业务流程。
- 允许工作站使用 GPU 硬件呈现。

## <a name="managed-servers-and-computers"></a>托管的服务器和计算机

记录的托管的服务器和计算机的名称。 可以在上找到此信息**主页**MultiPoint 管理器中的选项卡。

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


## <a name="stations"></a>工作站

记录本地工作站和其设置。 可以在上找到此信息**工作站**MultiPoint 管理器中的选项卡。

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

将复制的管理员和 MultiPoint 仪表板用户的用户名。 可以在上找到此信息**用户**MultiPoint 管理器中的选项卡。

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

记录你的 MultiPoint Services 部署 VDI 模板信息和虚拟机的名称。 可以在上找到此信息**虚拟桌面**MultiPoint 管理器中的选项卡。

**VDI 模板位置**: 

| # | 虚拟桌面的名称      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |