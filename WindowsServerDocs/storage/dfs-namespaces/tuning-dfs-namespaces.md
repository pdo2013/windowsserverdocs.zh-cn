---
title: "调整 DFS 命名空间"
description: "本文介绍如何调整或优化 DFS 命名空间。"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4614441fc54913ba5a8b547bbf1ad3e8ce7ee69b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="tuning-dfs-namespaces"></a>调整 DFS 命名空间

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

在创建命名空间并添加文件夹和目标后，请参考以下部分调整或优化 DFS 命名空间处理应用和轮询 Active Directory 域服务 (AD DS) 以获得更新的命名空间数据的方式：

-   [对命名空间启用基于访问的枚举](enable-access-based-enumeration-on-a-namespace.md)
-   [启用或禁用引用和客户端故障回复](enable-or-disable-referrals-and-client-failback.md)
-   [更改客户端缓存引用的时间](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [为引用中的目标设置排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   [设置目标优先级以替代引用排序](set-target-priority-to-override-referral-ordering.md)
-   [优化命名空间轮询](optimize-namespace-polling.md)
-   [使用继承的权限执行基于访问的枚举](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> 若要搜索文件夹或文件夹目标，请选择命名空间，单击**搜索**选项卡，并在文本框中键入搜索字符串，然后单击**搜索**。