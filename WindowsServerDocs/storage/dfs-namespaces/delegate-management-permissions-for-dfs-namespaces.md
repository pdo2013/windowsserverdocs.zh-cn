---
title: "委派 DFS 命名空间的管理权限"
description: "本文介绍如何委派 DFS 命名空间的管理权限，以及哪些组可以默认执行命名空间任务"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e584b49639a83e4ab1da142a999741ae4ac7ff84
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="delegate-management-permissions-for-dfs-namespaces"></a>委派 DFS 命名空间的管理权限

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

下表介绍可以默认执行基本命名空间任务的组，以及委派能够执行这些任务的方法：

|任务 | 可以默认执行此任务的组 | 委派方法 |
|---|---|---|
|创建基于域的命名空间|配置了命名空间的域中的域管理员组|右键单击控制台树中的**命名空间**节点，然后单击**委派管理权限**。 或者，使用 [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) 和 [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot)。 Windows PowerShell cmdlet（在 Windows Server 2012 中引入）。 你还必须将用户添加到命名空间服务器上的本地管理员组。|
|将命名空间服务器添加到基于域的命名空间|配置了命名空间的域中的域管理员组| 右键单击控制台树中的基于域的命名空间，然后单击**委派管理权限**。 或者，使用 [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) 和 [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot)。 Windows PowerShell cmdlet（在 Windows Server 2012 中引入）。 你还必须将用户添加到要添加的命名空间服务器上的本地管理员组。|
|管理基于域的命名空间|每个命名空间服务器上的本地管理员组| 右键单击控制台树中的基于域的命名空间，然后单击**委派管理权限**。 |
|创建独立命名空间|命名空间服务器上的本地管理员组| 将用户添加到命名空间服务器上的本地管理员组。 |
|管理独立命名空间*|命名空间服务器上的本地管理员组| 右键单击控制台树中的独立命名空间，然后单击**委派管理权限**。 或者，使用 [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) 和 [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot)。 Windows PowerShell cmdlet（在 Windows Server 2012 中引入）。|
|创建复制组或对文件夹启用 DFS 复制|配置了命名空间的域中的域管理员组| 右键单击控制台树中的“复制”节点，然后单击**委派管理权限**。 |

<br />

\*如果委派管理权限以管理独立命名空间，则用户无法使用**委派**选项卡查看和管理安全性，除非用户属于命名空间服务器上的本地管理员组。 之所以出现此问题，原因是 DFS 管理单元无法检索自定义访问控制列表 (DACL)，以从注册表中获得独立命名空间。 若要启用该管理单元以显示委派信息，必须按照 Microsoft<sup>®</sup> 知识库文章 [KB314837：如何管理对注册表的远程访问](http://go.microsoft.com/fwlink?linkid=46803)中的步骤操作