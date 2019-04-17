---
title: "广告林恢复-验证复制"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adfs
ms.openlocfilehash: d85336a10e808755677a99777af8ecf5c07b05ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="resources-to-verify-replication-is-working"></a>资源验证复制正在运行 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
 
 恢复或重新安装所有 Dc 后，你可以通过来验证广告 DS 和 SYSVOL 是否已恢复并且复制正确使用**repadmin /replsum**，其中运行任何版本的 Windows Server。  
  
> [!TIP]
>  你还可以下载并运行[Active Directory 复制状态工具](https://www.microsoft.com/download/details.aspx?id=30005)(ADReplStatus，) 的免费工具，监视复制状态 Dc 和报告的错误。 ADReplStatus 需要尚不存在的情况下安装的.NET Framework 4。  
  
 检查 DFS 复制事件查看日志中为事件 ID 4602（或文件复制服务事件 ID 13516），这表明初始化 SYSVOL。  
  
 如果第一个恢复直流将记录事件 ID 4614（"域控制器正在等待执行初始复制。 复制的文件夹之前将保留在初始的同步状态复制其合作伙伴使用了"）DFS 复制日志中，然后事件 ID 4602 未显示，你需要执行以下手动步骤来恢复 DFSR 复制 SYSVOL:  
  
1.  如果 DFSR 事件 4612 显示在第一个还原域控制器上执行手动权威还原中所述[2218556：如何为（如"D4 /d2"的 FRS) 的 DFSR 复制 SYSVOL 强制权威和非授权同步](https://support.microsoft.com/kb/2218556)（https://support.microsoft.com/kb/2218556)。  
  
2.  设置**SysvolReady 标志**为 1 手动，述[947022 NETLOGON 共享，不会在你安装新完整或只读 Windows Server 2008 基于域控制器上的 Active Directory 域服务后](https://support.microsoft.com/kb/947022)。  
  
 你也可以创建诊断报告 DFS 复制。 有关详细信息，请参阅[创建 DFS 复制诊断报告](https://technet.microsoft.com/library/cc754227.aspx)和[DFS Step-by-Step 适用于 Windows Server 2008 指南](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx)。 如果该服务器运行的 Windows Server 2008 R2，你可以使用[dfsrdiag.exe ReplicationState 命令行切换](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx)。  
  
 你也可以运行使用 dcdiag.exe 复制错误检查复制测试。 有关详细信息，请参阅知识库[文章 249256](https://support.microsoft.com/kb/249256)。

## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
