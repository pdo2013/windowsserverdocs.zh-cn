---
title: "使用 DFS 复制对文件夹目标进行复制"
description: "本文介绍如何使用 DFS 复制对文件夹目标进行复制"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ef13c338d8b13c24a02efb0468f06d5fef803cea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>使用 DFS 复制对文件夹目标进行复制

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

你可以使用 DFS 复制使文件夹目标的内容保持同步，以便用户可以看到相同的文件，无论将客户端计算机引用到哪个文件夹目标，都是如此。

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>使用 DFS 复制对文件夹目标进行复制

1.  单击**开始**，指向**管理工具**，然后单击 **DFS 管理**。

2.  在控制台树中的**命名空间**节点下，右键单击包含两个或多个文件夹目标的文件夹，然后单击**复制文件夹**。

3.  按照“复制文件夹向导”中的说明操作。

> [!NOTE]
> 配置更改不会立即应用到所有成员，除非使用 [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) 和 [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup) cmdlet。 必须将新配置复制到所有域控制器，并且复制组中的每个成员都必须轮询其最近的域控制器，以获取更改。 此过程所需的时间取决于 Active Directory 目录服务 (AD DS) 复制延迟，以及每个成员上的长轮询时间间隔（60 分钟）。 若要立即轮询以获得配置更改，请打开命令提示符窗口，然后为复制组中的每个成员键入一次以下命令： <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
若要从 Windows PowerShell 会话执行此操作，请使用 [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) cmdlet（在 Windows Server 2012 R2 中引入）。

## <a name="see-also"></a>另请参阅

-   [部署 DFS 命名空间](deploying-dfs-namespaces.md)
-   [委派 DFS 命名空间的管理权限](delegate-management-permissions-for-dfs-namespaces.md)
-   [复制](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)