---
title: "创建 DFS 命名空间"
description: "本文介绍如何创建 DFS 命名空间。"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b01a83f165e0ef01c6413dcf8785435c8f3aca5a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-dfs-namespace"></a>创建 DFS 命名空间

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

若要创建新的命名空间，可以使用服务器管理器，在安装 DFS 命名空间角色服务时创建该命名空间。 你也可以从 Windows PowerShell 会话使用 [New-DfsnRoot cmdlet](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot)。 

Windows Server 2012 中引入了 DFSN Windows PowerShell 模块。 

或者，你可以按照以下过程在安装了角色服务后创建命名空间。

## <a name="to-create-a-namespace"></a>创建命名空间

1.  单击**开始**，指向**管理工具**，然后单击 **DFS 管理**。

2.  在控制台树中，右键单击**命名空间**节点，然后单击**新建命名空间**。

3.  按照**新建命名空间向导**中的说明操作。

    若要在故障转移群集上创建独立命名空间，请在**新建命名空间向导**的**命名空间服务器**页上，指定群集文件服务器实例的名称。

> [!IMPORTANT]
> 请不要尝试使用 Windows Server 2008 模式创建基于域的命名空间，除非林功能级别是 Windows Server 2003 或更高版本。 否则，可能会导致你无法从命名空间中删除 DFS 文件夹，同时生成以下错误消息：“无法删除该文件夹。 此函数无法完成。”

## <a name="see-also"></a>另请参阅

-   [部署 DFS 命名空间](deploying-dfs-namespaces.md)
-   [选择命名空间类型](choose-a-namespace-type.md)
-   [将命名空间服务器添加到基于域的 DFS 命名空间](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [委派 DFS 命名空间的管理权限](delegate-management-permissions-for-dfs-namespaces.md)。


