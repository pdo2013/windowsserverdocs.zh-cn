---
title: 设置目标优先级以替代引用排序
description: 本文介绍如何设置目标优先级以替代引用排序
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 59db08d5ef46b696f550a5fa0738c5c1f9375fda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826358"
---
# <a name="set-target-priority-to-override-referral-ordering"></a>设置目标优先级以替代引用排序

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

引用是在用户访问命名空间中包含目标的命名空间根路径或文件夹时，客户端计算机从域控制器或命名空间服务器接收的目标的排序列表。 引用中每个目标根据命名空间根路径或文件夹的排序方法进行排序。 

若要优化目标的排序方式，可为各个目标设置优先级。 例如，可以指定该目标是所有目标中的第一项、所有目标中的最后一项，或成本相等的所有目标中的第一项或最后一项。

## <a name="to-set-target-priority-on-a-root-target-for-a-domain-based-namespace"></a>为基于域的命名空间的根目标设置目标优先级的步骤

若要为基于域的命名空间中的根目录目标设置目标优先级，请按照以下过程操作：

1.  单击“开始”、指向“管理工具”，然后单击“DFS 管理”。

2.  在控制台树中的**命名空间**节点下，单击要为其根目录目标设置优先级的基于域的命名空间。

3.  在**详细信息**窗格中的**命名空间服务器**选项卡上，右键单击要更改其优先级的根目录目标，然后单击**属性**。

4.  在**高级**选项卡上，单击**替代引用排序**，然后单击所需的优先级。

    -   **所有目标中的第一项** 指定如果目标可用，用户应始终被引用到此目标。
    -   **所有目标中的最后一项** 指定用户绝不被引用到此目标，除非所有其他目标不可用。
    -   **同等开销目标中的第一项** 指定用户应被引用到成本相等的其他目标（通常指同一站点中的其他目标）之前的此目标。
    -   **成本相等的目标中的最后一项** 指定如果存在成本相等的其他目标（通常指同一站点中的其他目标），用户绝不被引用到此目标。

## <a name="to-set-target-priority-on-a-folder-target"></a>为文件夹目标设置目标优先级的步骤

若要为文件夹目标设置目标优先级，请按照以下过程操作：

1.  单击“开始”、指向“管理工具”，然后单击“DFS 管理”。

2.  在控制台树中的**命名空间**节点下，单击要为其设置优先级的目标的文件夹。

3.  在**详细信息**窗格中的**文件夹目标**选项卡上，右键单击要更改其优先级的文件夹目标，然后单击**属性**。

4.  在**高级**选项卡上，单击**替代引用排序**，然后单击所需的优先级。

> [!NOTE]
> 若要使用 Windows PowerShell 设置目标优先级，请将 [Set-DfsnRootTarget](https://technet.microsoft.com/library/jj884266.aspx) 和 [Set-DfsnFolderTarget](https://technet.microsoft.com/library/jj884264.aspx) cmdlet 与 **ReferralPriorityClass** 和 **ReferralPriorityRank** 参数结合使用。 Windows Server 2012 中引入了这些 cmdlet。

## <a name="see-also"></a>请参阅

-   [调试 DFS 命名空间](tuning-dfs-namespaces.md)
-   [为 DFS 命名空间委派管理权限](delegate-management-permissions-for-dfs-namespaces.md)