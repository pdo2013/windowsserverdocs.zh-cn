---
title: 使用 DFS 复制对文件夹目标进行复制
description: 本文介绍如何使用 DFS 复制对文件夹目标进行复制
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ad26b8685539869e9302eafac69df19d11778a1b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386133"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>使用 DFS 复制对文件夹目标进行复制

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows server 2008

你可以使用 DFS 复制使文件夹目标的内容保持同步，以便用户可以看到相同的文件，无论将客户端计算机引用到哪个文件夹目标，都是如此。

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>使用 DFS 复制对文件夹目标进行复制

1.  单击“开始”、指向“管理工具”，然后单击“DFS 管理”。

2.  在控制台树中的**命名空间**节点下，右键单击包含两个或多个文件夹目标的文件夹，然后单击**复制文件夹**。

3.  按照“复制文件夹向导”中的说明操作。

> [!NOTE]
> 配置更改不会立即应用到所有成员，除非使用 [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) 和 [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup) cmdlet。 必须将新配置复制到所有域控制器中，且复制组中的每个成员必须轮询其最近的域控制器，以获取更改。 此操作所需的时间取决于每个成员的 Active Directory 目录服务（AD DS）复制延迟和长轮询间隔（60分钟）。 若要立即轮询以获得配置更改，请打开命令提示符窗口，然后为复制组中的每个成员键入一次以下命令： <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
若要在 Windows PowerShell 会话中执行此操作，请使用 Windows Server 2012 R2 中引入的[DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) cmdlet。

## <a name="see-also"></a>请参阅

-   [部署 DFS 命名空间](deploying-dfs-namespaces.md)
-   [委派 DFS 命名空间的管理权限](delegate-management-permissions-for-dfs-namespaces.md)
-   [DFS 复制](../dfs-replication/dfsr-overview.md)