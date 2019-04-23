---
title: 调整 DFS 命名空间
description: 本文介绍如何调整或优化 DFS 命名空间。
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c11bbf65c3baebebe1e5143a5e694ca752500aca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844628"
---
# <a name="tuning-dfs-namespaces"></a>调整 DFS 命名空间

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

创建一个命名空间并添加文件夹和目标之后，请参阅以下各节中，若要调试或优化方式 DFS Namespace 处理引用以及轮询更新的命名空间数据的 Active Directory 域服务 (AD DS):

-   [启用基于访问权限的 Namespace 枚举](enable-access-based-enumeration-on-a-namespace.md)
-   [启用或禁用引用以及客户端故障回复](enable-or-disable-referrals-and-client-failback.md)
-   [更改客户端缓存引用的时间量](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [引用中的目标设置排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   [设置目标优先级来覆盖引用排序](set-target-priority-to-override-referral-ordering.md)
-   [优化 Namespace 轮询](optimize-namespace-polling.md)
-   [基于访问的枚举中使用继承的权限](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> 若要搜索文件夹或文件夹目标，请选择命名空间，单击**搜索**选项卡，并在文本框中键入搜索字符串，然后单击**搜索**。