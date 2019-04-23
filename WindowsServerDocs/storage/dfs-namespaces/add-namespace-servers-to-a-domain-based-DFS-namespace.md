---
title: 将命名空间服务器添加到基于域的 DFS 命名空间
description: 本文介绍如何指定其他命名空间服务器，以使用 DFS 管理托管命名空间。
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: bb3b98e1ea687b68bbb87d0da413f9624d336370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853328"
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>将命名空间服务器添加到基于域的 DFS 命名空间

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

你可以指定要托管命名空间的其他命名空间服务器，以增加基于域的命名空间的可用性。

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>将命名空间服务器添加到基于域的命名空间

若要使用 DFS 管理将命名空间服务器添加到基于域的命名空间，请按照以下过程操作：

1.  单击“开始”、指向“管理工具”，然后单击“DFS 管理”。

2.  在控制台树中的**命名空间**节点下，右键单击基于域的命名空间，然后单击**添加命名空间服务器**。

3.  输入其他服务器的路径，或单击**浏览**以找到服务器。

> [!NOTE]
> 此过程不适用于独立命名空间，因为这些命名空间仅支持单个命名空间服务器。 若要增加独立命名空间的可用性，请在“新建命名空间向导”中，将故障转移群集指定为命名空间服务器。


> [!TIP]
> 若要使用 Windows PowerShell 添加命名空间服务器，请使用 [New-DfsnRootTarget cmdlet](https://docs.microsoft.com/powershell/module/dfsn/set-dfsnroottarget)。 Windows Server 2012 中引入了 DFSN Windows PowerShell 模块。

## <a name="see-also"></a>请参阅

-   [部署 DFS 命名空间](deploying-dfs-namespaces.md)
-   [复查 DFS 命名空间服务器要求](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [创建 DFS Namespace](create-a-dfs-namespace.md)
-   [为 DFS 命名空间委派管理权限](delegate-management-permissions-for-dfs-namespaces.md)

