---
title: 在 DFS 命名空间中创建文件夹
description: 本文介绍如何在 DFS 命名空间中创建文件夹。
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7389b825afe5ccae3059f50ffdedac72ecd5ac9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402245"
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>在 DFS 命名空间中创建文件夹

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows Server 2008

你可以使用文件夹在命名空间中创建其他级别的层次结构。 你也可以创建包含文件夹目标的文件夹，以将共享文件夹添加到命名空间。 包含文件夹目标的 DFS 文件夹不能包含其他 DFS 文件夹，因此，如果你要向命名空间添加层次结构级别，请不要将文件夹目标添加到文件夹。

按照以下过程使用 DFS 管理在命名空间中创建文件夹：

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>在 DFS 命名空间中创建文件夹

1.  单击“开始”、指向“管理工具”，然后单击“DFS 管理”。

2.  在控制台树中的**命名空间**节点下，右键单击命名空间或命名空间中的文件夹，然后单击**新建文件夹**。

3.  在**名称**文本框中，键入新文件夹的名称。

4.  若要向文件夹添加一个或多个文件夹目标，请单击**添加**，并指定文件夹目标的通用命名约定 (UNC) 路径，然后单击**确定**。


> [!TIP]
> 若要使用 Windows PowerShell 在命名空间中创建文件夹，请使用 [New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder) cmdlet。 Windows Server 2012 中引入了 DFSN Windows PowerShell 模块。


## <a name="see-also"></a>请参阅

-   [部署 DFS 命名空间](deploying-dfs-namespaces.md)
-   [委派 DFS 命名空间的管理权限](delegate-management-permissions-for-dfs-namespaces.md)


