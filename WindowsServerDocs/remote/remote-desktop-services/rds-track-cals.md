---
title: 跟踪远程桌面服务客户端访问许可证 (RDS CAL)
description: 了解如何跟踪整个 RDS 部署中的 CAL。
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
ms.openlocfilehash: a3b106a23660e1608231623bfd048669d97f9719
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712014"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>跟踪远程桌面服务客户端访问许可证 (RDS CAL)

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

可以使用远程桌面授权管理器工具创建报告，用于跟踪远程桌面许可证服务器颁发的 RDS 基于用户的 CAL。

> [!NOTE]
>  如果在环境中使用 Azure AD 域服务，则无法使用远程桌面授权管理器工具来获取基于用户的 CAL。 在这种情况下，需要通过登录事件、轮询通过连接代理建立的活动远程桌面连接或适用的其他机制手动跟踪授权。 

使用以下步骤生成基于用户的 CAL 报告：

1. 在远程桌面授权管理器中右键单击许可证服务器，然后依次单击“创建报告”、“基于用户的 CAL 使用情况”。  
2. 设置报告范围 - 选择以下选项之一：
   - 整个域 - 许可证服务器所属的域。
   - 组织单位 - 许可证服务器所属的域中的任意 OU。
   - 整个域和所有受信任的域 - 可以包含其他林中的域。 选择此选项可能会增加创建报告所需的时间。

   所做的选择决定了要在 AD DS 中的哪些用户帐户内，搜索用于生成报告的 RDS 基于用户的 CAL 信息。
3. 单击“创建报告”。  随后会创建报告，并显示一条消息确认已成功创建报告。 单击“确定”  关闭显示的消息。

创建的报告将显示在许可证服务器节点下的“报告”部分。 该报告提供以下信息：

- 报告的创建日期和时间
- 报告范围（例如“域”、“OU=Sales”或“所有受信任的域”）
- 在许可证服务器上安装的 RDS 基于用户的 CAL 数目
- 许可证服务器根据特定的报告范围颁发的 RDS 基于用户的 CAL 数目

还可以将此报告作为 CSV 文件保存到计算机上的某个文件夹位置。 若要保存报告，请右键单击该报告，单击“另存为”，然后指定用于保存该报告的文件名和位置。

创告的报告将列在远程桌面授权管理器中许可证服务器节点下的“报告”节点中。 如果不再需要报告，可将其删除。
