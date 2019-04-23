---
title: 跟踪远程桌面服务客户端访问许可证 (RDS Cal)
description: 了解如何在 RDS 部署之间跟踪 Cal。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80d82d30-3ad0-4a8c-9a9b-2773c47eee19
author: lizap
ms.author: elizapo
ms.date: 05/11/2017
manager: dongill
ms.openlocfilehash: ed7490b2b61eb516d43b280b67fcefaeedb3e225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890618"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>跟踪远程桌面服务客户端访问许可证 (RDS Cal)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

远程桌面授权管理器工具可用于创建报表来跟踪 rds-每用户 Cal 的已颁发的远程桌面许可证服务器。

> [!NOTE]
>  如果在环境中使用 Azure AD 域服务，远程桌面授权管理器工具不起作用，以获取每用户 Cal。 相反，您需要跟踪许可手动，通过登录事件、 轮询活动的远程桌面连接，而通过连接代理或适用于你的另一种机制。 

使用以下步骤来生成每个用户 Cal 报表：

1. 在远程桌面授权管理器中右键单击许可证服务器，请单击**创建报表**，然后单击**每用户 CAL 使用情况**。
2. 设置报表作用域-选择以下项之一：
   - 整个域的许可证服务器所属域。
   - 组织单位的许可证服务器所属域内的任意 OU。
   - 整个域和所有受信任的域-可以包括在其他林中的域。 选择此选项可以增加创建报表所需的时间。

   所做的选择将确定 RDS 每用户 CAL 的信息来生成报表中搜索 AD DS 中的哪些用户帐户。
3. 单击**创建报表**。 创建报表并显示一条消息，以确认已成功创建报表。 单击“确定”关闭显示的消息。

在许可证服务器节点下的报告部分中将显示你创建的报表。 该报告提供以下信息：

- 创建报表的日期和时间
- 报告的范围 (例如域、 OU = Sales 或所有受信任的域)
- Rds-每用户 Cal 的许可证服务器上安装的数量
- Rds-每用户 Cal 已颁发的许可证服务器特定于报表的作用域的数量

此外可以为 CSV 文件到计算机上的文件夹位置保存报表。 若要保存报表，右键单击你想要保存，单击另存为，然后指定文件的名称和位置来保存报表的报表。

用于远程桌面授权管理器中的许可证服务器节点下的报表节点中列出了你创建的报表。 如果不再需要报表，则可以将其删除。
