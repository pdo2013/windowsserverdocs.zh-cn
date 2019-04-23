---
title: 使用 DFS 复制对文件夹目标进行复制
description: 本文介绍如何使用 DFS 复制对文件夹目标进行复制
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 098ffd1a47891d2355742682a002f80dcfc59650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878078"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>使用 DFS 复制对文件夹目标进行复制

> 适用于：Windows Server 2019、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008

你可以使用 DFS 复制使文件夹目标的内容保持同步，以便用户可以看到相同的文件，无论将客户端计算机引用到哪个文件夹目标，都是如此。

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>使用 DFS 复制对文件夹目标进行复制

1.  单击“开始”、指向“管理工具”，然后单击“DFS 管理”。

2.  在控制台树中的**命名空间**节点下，右键单击包含两个或多个文件夹目标的文件夹，然后单击**复制文件夹**。

3.  按照“复制文件夹向导”中的说明操作。

> [!NOTE]
> 配置更改不会立即应用到所有成员，除非使用 [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) 和 [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup) cmdlet。 必须将新配置复制到所有域控制器中，且复制组中的每个成员必须轮询其最近的域控制器，以获取更改。 花费的时间量取决于 Active Directory 目录服务 (AD DS) 复制延迟以及长时间每个成员上的轮询间隔 （60 分钟）。 若要立即轮询以获得配置更改，请打开命令提示符窗口，然后为复制组中的每个成员键入一次以下命令： <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
若要执行此操作在 Windows PowerShell 会话中，使用[更新 DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) cmdlet，后者在 Windows Server 2012 R2 中引入。

## <a name="see-also"></a>请参阅

-   [部署 DFS 命名空间](deploying-dfs-namespaces.md)
-   [为 DFS 命名空间委派管理权限](delegate-management-permissions-for-dfs-namespaces.md)
-   [DFS 复制](../dfs-replication/dfsr-overview.md)