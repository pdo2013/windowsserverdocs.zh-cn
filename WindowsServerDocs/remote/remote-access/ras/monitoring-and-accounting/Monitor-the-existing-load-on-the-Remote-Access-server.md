---
title: 监视远程访问服务器上的现有负载
description: 本主题是指南的适用于远程访问监视和记帐 Windows Server 2016 中的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1f47273ab3be6faa762df2fb90d6486bc0ed2d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849018"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>监视远程访问服务器上的现有负载

>适用于：Windows 服务器 （半年频道），Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
术语**负载**指的是与远程访问服务器上的连接数相关的统计信息。 下面是跟踪远程访问服务器上的负载所需的步骤。  
  
可以使用在管理控制台中查看服务器的负载统计信息在远程访问服务器上可用的监视仪表板，也可以使用性能监视器计数器来跟踪统计信息。  
  
> [!NOTE]  
> 您必须登录以 Domain Admins 组的成员或每台计算机上 Administrators 组的成员才能完成本主题中所述的任务。 如果已使用该帐户是 Administrators 组的成员登录时，无法完成任务，请尝试执行该任务，而是 Domain Admins 组的成员帐户登录。  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>若要使用在监视仪表板来监视远程访问服务器负载  
  
1.  在“服务器管理器”中，单击“工具”，然后单击“远程访问管理”。  
  
2.  单击“仪表板”以导航到“远程访问管理控制台”中的“远程访问仪表板”。  
  
3.  在监视仪表板，请注意**远程客户端状态**内磁贴**服务器状态**磁贴。 此磁贴列出的远程连接的客户端总数、 已连接的 DirectAccess 客户端的总数和的最大连接过去 24 小时内的用户数等统计信息。  
  
4.  可以单击**刷新**下**任务**在右窗格中重新加载的运行状况状态。 若要更改默认刷新间隔，请单击**配置刷新间隔**下**任务**。  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>若要使用性能监视器工具以监视远程访问服务器上的性能计数器  
  
1.  单击**启动**，单击**管理工具**，然后双击**性能监视器**。  
  
2.  下**性能**，单击**性能监视器**。  
  
3.  单击**外**中 （以绿色的十字图标表示） 的按钮**性能监视器**工具栏。  
  
4.  从列表中的**可用的计数器**，选择中的所有计数器**RAS**并**RAmgmtsvc**类别，并单击**添加 >>**.  
  
5.  同样，从列表中**可用的计数器**，选择中的所有计数器**IPsec 连接**类别中，并单击**添加 >>。**  
  
6.  单击**确定**以添加在所选的计数器**性能监视器**控制台进行跟踪。  
  
**性能监视器**现在将以图形方式显示所选的服务器负载统计信息。  
  
![Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


