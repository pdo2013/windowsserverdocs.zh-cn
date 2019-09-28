---
title: AD 林恢复-验证复制
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: f65508bf8973721a09c779a52f708d6a258e2cb5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390228"
---
# <a name="resources-to-verify-replication-is-working"></a>用于验证复制的资源正在运行 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

还原或重新安装所有 Dc 后，可以使用运行在任意版本的 Windows Server 上的**repadmin/replsum**来验证 AD DS 和 SYSVOL 是否已恢复并正确复制。  
  
> [!TIP]
> 你还可以下载并运行[Active Directory 复制状态工具](https://www.microsoft.com/download/details.aspx?id=30005)（ADReplStatus），它是一种可监视 dc 复制状态和报告错误的免费工具。 ADReplStatus 需要 .NET Framework 4，如果它尚不存在，则将安装它。  

在事件查看器中检查事件 ID 4602 （或文件复制服务事件 ID 13516）的 DFS 复制登录，这表明 SYSVOL 已初始化。  

如果第一个恢复的 DC 记录事件 ID 4614 （"域控制器正在等待执行初始复制。 在 DFS 复制日志中，已复制的文件夹将保持初始同步状态，直到它与其伙伴复制 "），然后不会出现事件 ID 4602，并且你需要执行以下手动步骤来恢复 SYSVOL （如果复制了该文件夹）DFSR  

1. 如果在第一个还原的 DC 上出现 DFSR 事件4612，则执行手动授权还原，如 [2218556：如何强制进行 DFSR 复制的 SYSVOL 的权威和非权威同步（如 "适用于 FRS 的 D4/D2"） ](https://support.microsoft.com/kb/2218556) （@no__t 为-1。  
2. 手动将**SysvolReady 标志**设置为1，如947022中所述，在[新的完整或只读的基于 Windows Server 2008 的域控制器上安装 ACTIVE DIRECTORY 域服务后，NETLOGON 共享不存在](https://support.microsoft.com/kb/947022)。  

您还可以 DFS 复制创建诊断报告。 有关详细信息，请参阅为 DFS 复制和[DFS 分步指南创建用于 Windows Server 2008 的](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx)[诊断报告](https://technet.microsoft.com/library/cc754227.aspx)。 如果服务器正在运行 Windows Server 2008 R2，则可以使用[Dfsrdiag.exe ReplicationState 命令行开关](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx)。  

你还可以使用 dcdiag.exe 运行复制测试来检查复制错误。 有关详细信息，请参阅知识库[文章 249256](https://support.microsoft.com/kb/249256)。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
