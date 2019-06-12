---
title: 为引用中的目标设置排序方法
description: 本文介绍如何为引用中的目标设置排序方法。
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 52568944a98bed7960b37335b2e3cbbde61479ca
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447205"
---
# <a name="set-the-ordering-method-for-targets-in-referrals"></a>为引用中的目标设置排序方法

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

引用是在用户访问命名空间根目录或包含目标的文件夹时，客户端计算机从域控制器或命名空间服务器接收的目标的排序列表。 收到引用后，客户端会尝试访问该列表中的第一个目标。 如果该目标不可用，则客户端会尝试访问下一个目标。
引用中始终先列出客户端站点上的目标。 客户端站点之外的目标根据排序方法列出。

参考以下部分，以指定应将目标应用到客户端的顺序，并了解对目标引用进行排序的不同方法：

## <a name="to-set-the-ordering-method-for-targets-in-namespace-root-referrals"></a>为命名空间根目录引用中的目标设置排序方法

按照以下过程为命名空间根目录设置排序方法：

1.  单击“开始”  、指向“管理工具”  ，然后单击“DFS 管理”  。

2.  在控制台树中的“命名空间”  节点下，右键单击命名空间，然后单击“属性”  。

3.  在**引用**选项卡上，选择排序方法。

> [!NOTE]
> 若要使用 Windows PowerShell 为命名空间根目录引用中的目标设置排序方法，请将 [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx) cmdlet 与以下参数之一结合使用：
>    -   **EnableSiteCosting** 指定**最低成本排序**方法
>    -   **EnableInsiteReferrals** 指定**排除客户端站点之外的目标**排序方法
>    -   无论省略哪个参数，都会指定**随机顺序**引用排序方法。 

在 Windows Server 2012 中引入的 DFSN Windows PowerShell 模块。
   
## <a name="to-set-the-ordering-method-for-targets-in-folder-referrals"></a>为文件夹引用中的目标设置排序方法

包含目标的文件夹从命名空间根目录继承排序方法。 你可以按照以下过程替代排序方法：

1.  单击“开始”  、指向“管理工具”  ，然后单击“DFS 管理”  。

2.  在控制台树中的“命名空间”  节点下，右键单击包含目标的文件夹，然后单击“属性”  。

3.  在**引用**选项卡上，选中**排除客户端站点之外的目标**复选框。

> [!NOTE]
> 若要使用 Windows PowerShell 排除客户端站点之外的文件夹目标，请使用 [Set-DfsnFolder –EnableInsiteReferrals](https://technet.microsoft.com/library/jj884283.aspx) cmdlet。

## <a name="target-referral-ordering-methods"></a>目标引用排序方法

三种排序方法如下：

-   随机顺序
-   最低成本
-   排除客户端站点之外的目标

## <a name="random-order"></a>随机顺序

如果采用这种方法，则目标的排序方式如下：

1.  在引用的顶部随机顺序列出客户端与位于同一 Active Directory 目录服务 (AD DS) 站点中的目标。
2.  客户端站点之外的目标按随机顺序列出。

如果同一站点的目标服务器不可用，则客户端计算机被引用到随机目标服务器，无论连接成本多高或距离目标多远，都是如此。

## <a name="lowest-cost"></a>最低成本

如果采用这种方法，则目标的排序方式如下：

1.  与客户端相同的站点中的目标按随机顺序列在引用的顶部。
2.  客户端站点之外的目标按成本从最低到最高的顺序列出。 成本相同的引用组合在一起，并且每个组中的目标按随机顺序列出。

> [!NOTE]
> 网站链接成本不显示在 DFS 管理单元中。 若要查看网站链接成本，请使用 Active Directory 站点和服务管理单元。

## <a name="exclude-targets-outside-of-the-clients-site"></a>排除客户端站点之外的目标

如果采用这种方法，引用中只包含与客户端相同的站点中的目标。 这些同一站点中的目标按随机顺序列出。 如果不存在同一站点中的目标，则客户端将不会收到引用，并且无法访问命名空间中的该部分。

> [!NOTE]
> 目标如果其目标优先级设置为“所有目标中的第一项”或“所有目标中的最后一项”，则仍在引用中列出，即使将排序方法设置为**排除客户端站点之外的目标**。

## <a name="see-also"></a>请参阅 

-   [调整 DFS 命名空间](tuning-dfs-namespaces.md)
-   [委派 DFS 命名空间的管理权限](delegate-management-permissions-for-dfs-namespaces.md)