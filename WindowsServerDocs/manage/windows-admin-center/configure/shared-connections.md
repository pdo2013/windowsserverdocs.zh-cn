---
title: 为 Windows 管理中心网关的所有用户配置共享连接
description: 了解管理员如何将 Windows 管理中心 (Project Honolulu) 网关配置为允许所有用户共享单个连接列表。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c632c178e91b92e0a80d8c72e8ea0ce3ce37b502
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357296"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>为 Windows 管理中心网关的所有用户配置共享连接

> 适用于：Windows 管理中心预览, Windows 管理中心

通过配置共享连接的功能, 网关管理员可以为给定 Windows 管理中心网关的所有用户配置连接列表一次。 

在 Windows 管理中心网关设置的 "**共享连接**" 选项卡上, 网关管理员可以从 "所有连接" 页添加服务器、群集和 PC 连接, 包括标记连接的功能。 在 "共享连接" 列表中添加的任何连接和标记将显示在 "所有连接" 页中, 用于此 Windows 管理中心网关的所有用户。
    ![](../media/shared-cnxns-1.png)

在配置共享连接后, 任何 Windows 管理中心用户访问 "所有连接" 页面时, 他们将看到其连接分为两部分:个人和共享连接。 个人组是特定用户的连接列表, 在该用户的浏览器会话中保持不变。 所有用户的共享连接组都是相同的, 不能从 "所有连接" 页修改。
![](../media/shared-cnxns-2.png)