---
title: 更改客户端缓存引用的时间
description: 本文介绍如何更改客户端缓存引用的时间
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c6fcf64dc15404ca59e3ce5552b258f782441cfb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402264"
---
# <a name="change-the-amount-of-time-that-clients-cache-referrals"></a>更改客户端缓存引用的时间

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows Server 2008

引用是在用户访问命名空间中包含目标的命名空间根路径或文件夹时，客户端计算机从域控制器或命名空间服务器接收的目标的排序列表。 你可以调整在请求新引用之前客户端缓存引用的时间。

## <a name="to-change-the-amount-of-time-that-clients-cache-namespace-root-referrals"></a>更改客户端缓存命名空间根路径引用的时间的步骤

1.  单击“开始”、指向“管理工具”，然后单击“DFS 管理”。

2.  在控制台树中的“命名空间”节点下，右键单击命名空间，然后单击“属性”。

3.  在“引用”选项卡上的“缓存持续时间（秒）”文本框中，键入客户端缓存命名空间根路径引用的时间（秒）。 默认设置为 300 秒（五分钟）。

> [!TIP]
> 若要使用 Windows PowerShell 更改客户端缓存命名空间根目录引用的时间，请使用 [Set-DfsnRoot TimeToLiveSec](https://technet.microsoft.com/library/jj884281.aspx) cmdlet。 这些 cmdlet 是在 Windows Server 2012 中引入的。

## <a name="to-change-the-amount-of-time-that-clients-cache-folder-referrals"></a>更改客户端缓存文件夹引用的时间的步骤

1.  单击**开始**，指向**管理工具**，然后单击 **DFS 管理**。

2.  在控制台树中的“命名空间”节点下，右键单击包含目标的文件夹，然后单击“属性”。

3.  在“引用”选项卡上的“缓存持续时间（秒）”文本框中，键入客户端缓存文件夹引用的时间（秒）。 默认设置为 1800 秒（30 分钟）。

## <a name="see-also"></a>请参阅

-   [调整 DFS 命名空间](tuning-dfs-namespaces.md)
-   [委派 DFS 命名空间的管理权限](delegate-management-permissions-for-dfs-namespaces.md)


