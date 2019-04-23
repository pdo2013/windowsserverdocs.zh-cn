---
title: AD 林恢复-确认复制
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: fb05586f281460dc2c7a1afea4c0423493e3fc46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878308"
---
# <a name="resources-to-verify-replication-is-working"></a>资源，以验证复制正常工作 

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

在还原或重新安装所有域控制器后，你可以验证 AD DS 和 SYSVOL 是否已恢复和复制正确使用**repadmin /replsum**，在任何版本的 Windows Server 上运行。  
  
> [!TIP]
> 此外可以下载并运行[Active Directory 复制状态工具](https://www.microsoft.com/download/details.aspx?id=30005)(ADReplStatus)，一个免费工具，可监视的域控制器和报告错误的复制状态。 ADReplStatus 需要.NET Framework 4 中，如果尚未存在将安装它。  

检查事件查看器事件 ID 4602 （或文件复制服务事件 ID 13516），指示已初始化了 SYSVOL 中的 DFS 复制日志。  

如果第一个恢复 DC 将记录事件 ID 4614 （"域控制器正在等待执行初始复制。 已复制的文件夹之前将保留在初始同步状态可以与其伙伴复制"） 中的 DFS 复制日志，则不显示事件 ID 4602 和需要执行以下手动步骤恢复 SYSVOL，如果由复制DFSR:  

1. 如果 DFSR 事件 4612 显示在第一个已还原域控制器上执行手动的权威还原，如中所述[2218556:如何强制进行 DFSR 复制的 SYSVOL （如"D4/D2"frs) 的权威和非权威同步](https://support.microsoft.com/kb/2218556)(https://support.microsoft.com/kb/2218556)。  
2. 设置**SysvolReady 标志**为 1 中所述手动[947022 NETLOGON 共享不存在新的完全或只读的基于 Windows Server 2008 的域控制器上安装 Active Directory 域服务后](https://support.microsoft.com/kb/947022).  

此外可以创建 DFS 复制诊断报告。 有关详细信息，请参阅[为 DFS 复制创建诊断报告](https://technet.microsoft.com/library/cc754227.aspx)并[DFS 分步指南为 Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx)。 如果服务器运行 Windows Server 2008 R2，则可以使用[dfsrdiag.exe ReplicationState 命令行开关](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx)。  

此外可以运行的复制测试使用 dcdiag.exe 的复制错误检查。 有关详细信息，请参阅知识库[一文 249256](https://support.microsoft.com/kb/249256)。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
