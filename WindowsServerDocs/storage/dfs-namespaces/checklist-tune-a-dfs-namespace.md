---
title: 清单：调整 DFS 命名空间
description: 本文介绍如何优化 DFS 命名空间处理引用和轮询 AD DS 以获得更新的命名空间数据的方式
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5e2d43f75a64a6a7950539c14386c6a037bc3c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872638"
---
# <a name="checklist-tune-a-dfs-namespace"></a>清单：优化 DFS 命名空间

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

创建命名空间并添加文件夹和目标，使用以下清单调试或优化的方式后 DFS 命名空间处理引用，并通过轮询 Active Directory 域服务 (AD DS) 检查已更新的命名空间数据。

-   阻止用户查看命名空间中他们无权访问的文件夹。 [启用基于访问权限的 Namespace 枚举](enable-access-based-enumeration-on-a-namespace.md) 
-   允许或阻止用户在访问命名空间中的文件夹时被引用到该命名空间或文件夹目标。 [启用或禁用引用以及客户端故障回复](enable-or-disable-referrals-and-client-failback.md) 
-   调整在请求新引用之前客户端缓存引用的时间。 [更改客户端缓存引用的时间量](change-the-amount-of-time-that-clients-cache-referrals.md)
-   优化命名空间服务器轮询 AD DS 以获取最新的命名空间数据的方式。 [优化 Namespace 轮询](optimize-namespace-polling.md)
-   使用继承的权限控制哪些用户可以查看命名空间中对其启用基于访问的枚举的文件夹。 [基于访问的枚举中使用继承的权限](using-inherited-permissions-with-access-based-enumeration.md)

此外，通过使用一个称为目标优先级的 DFS 命名空间增强功能，您可以指定服务器的优先级，以便特定服务器始终位于第一次或最后一个服务器 （称为引用） 的列表客户端接收时访问的文件夹包含命名空间中的目标。

-   指定应将用户引用到文件夹目标的顺序。 [引用中的目标设置排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   替代特定命名空间服务器或文件夹目标的引用排序。 [设置目标优先级来覆盖引用排序](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>请参阅

-   [命名空间](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [清单：部署 DFS 命名空间](checklist-deploy-dfs-namespaces.md)
-   [调试 DFS 命名空间](tuning-dfs-namespaces.md)


