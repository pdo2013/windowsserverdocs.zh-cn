---
title: 优化命名空间轮询
description: 本文介绍如何优化命名空间轮询，以使基于域的命名空间在命名空间服务器之间保持一致
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 995b01604b680746c4b0d6502b3b3968503d4210
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878268"
---
# <a name="optimize-namespace-polling"></a>优化命名空间轮询

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

若要使基于域的命名空间在命名空间服务器之间保持一致，命名空间服务器需要定期轮询 Active Directory 域服务 (AD DS)，以获取最新的命名空间数据。 

## <a name="to-optimize-namespace-polling"></a>优化命名空间轮询的步骤

按照以下过程优化命令空间轮询的执行方式：

1.  单击“开始”、指向“管理工具”，然后单击“DFS 管理”。

2.  在控制台树中的**命名空间**节点下，右键单击基于域的命名空间，然后单击**属性**。

3.  在**高级**选项卡上，选择是否需要针对一致性或可伸缩性优化的命名空间。

    -   如果承载命名空间的命名空间服务器不超过 16 个，则选择**针对一致性优化**。
    -   如果命名空间服务器超过 16 个，则选择**优化可伸缩性**。 这会减少主域控制器 (PDC) 模拟器上的负载，但会增加将对命名空间所做的更改复制到所有命名空间服务器所需的时间。 在将更改复制到所有服务器之前，用户可能会得到该命名空间不一致的视图。

> [!NOTE]
> 若要使用 Windows PowerShell 中设置的命名空间轮询模式，使用[集 DfsnRoot EnableRootScalability](https://technet.microsoft.com/library/jj884281.aspx) cmdlet，后者在 Windows Server 2012 中引入。

## <a name="see-also"></a>请参阅

-   [调试 DFS 命名空间](tuning-dfs-namespaces.md)
-   [为 DFS 命名空间委派管理权限](delegate-management-permissions-for-dfs-namespaces.md)