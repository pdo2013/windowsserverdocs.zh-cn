---
Title: 将 SYSVOL 复制迁移到 DFS 复制
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7ea883730fcab83d064fa41f610bde4837510276
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836638"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>将 SYSVOL 复制迁移到 DFS 复制


更新日期：2010 年 8 月 25日日

适用于：Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008

域控制器使用名为 SYSVOL 的特殊共享的文件夹复制到其他域控制器的登录脚本和组策略对象文件。 Windows 2000 Server 和 Windows Server 2003 使用文件复制服务 (FRS) 复制 SYSVOL，而 Windows Server 2008 使用更高版本时使用 Windows Server 2008 域功能级别的域中的 DFS 复制服务和域的 FRS 的运行较旧的域功能级别。

若要使用 DFS 复制来复制 SYSVOL 文件夹，您可以创建一个新域，使用 Windows Server 2008 域功能级别，或可以使用来升级现有域和迁移到复制本文档中讨论的过程DFS 复制。

本文档假定您具有的 Active Directory 域服务 (AD DS)、 FRS 和分布式文件系统复制 （复制） 的基本知识。 有关详细信息，请参阅[Active Directory 域服务概述](http://go.microsoft.com/fwlink/?linkid=147787)， [FRS 概述](http://go.microsoft.com/fwlink/?linkid=121763)，或[DFS 复制概述](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> 若要下载本指南的可打印版本，请转到<a href="http://go.microsoft.com/fwlink/?linkid=150375">SYSVOL 复制迁移指南：FRS 到 DFS 复制</a>(http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>本指南包含的内容

[SYSVOL 迁移的概念性信息](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [SYSVOL 迁移状态](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [SYSVOL 迁移过程的概述](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[SYSVOL 迁移过程](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [迁移到准备就绪状态](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [迁移到重定向状态](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [迁移到已清除状态](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[SYSVOL 迁移疑难解答](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [SYSVOL 迁移问题疑难解答](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [回滚到以前的稳定状态的 SYSVOL 迁移](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[SYSVOL 迁移的参考信息](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [受支持的 SYSVOL 迁移方案](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [验证 SYSVOL 迁移状态](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [SYSVOL 迁移工具操作](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>其他参考

[SYSVOL 迁移系列：第 1 部分-简介 SYSVOL 迁移过程](http://go.microsoft.com/fwlink/?linkid=121756)

[SYSVOL 迁移系列：一部分 2—Dfsrmig.exe:SYSVOL 迁移工具](http://go.microsoft.com/fwlink/?linkid=121757)

[SYSVOL 迁移系列：第 3 部分-迁移到准备状态](http://go.microsoft.com/fwlink/?linkid=121758)

[SYSVOL 迁移系列：第 4 部分-迁移到 REDIRECTED 状态](http://go.microsoft.com/fwlink/?linkid=121759)

[SYSVOL 迁移系列：第 5 部分-迁移到消除状态](http://go.microsoft.com/fwlink/?linkid=121760)

[对于 Windows Server 2008 中的分布式的文件系统的分步指南](http://go.microsoft.com/fwlink/?linkid=85231)

[FRS 技术参考](http://go.microsoft.com/fwlink/?linkid=121764)

