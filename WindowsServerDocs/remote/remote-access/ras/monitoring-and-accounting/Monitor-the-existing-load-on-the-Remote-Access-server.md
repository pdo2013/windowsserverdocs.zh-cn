---
title: 监视远程访问服务器上的现有负载
description: 本主题是 Windows Server 2016 中的远程访问监视和记帐指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43c447205a5ef0cbd33b0486e01d630e6d00c633
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367217"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>监视远程访问服务器上的现有负载

>适用于：Windows Server（半年频道）、Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
术语 "**加载**" 指的是与远程访问服务器上的连接数相关的统计信息。 下面是在远程访问服务器上跟踪负载所需的步骤。  
  
你可以使用远程访问服务器上的管理控制台中提供的监视仪表板来查看服务器的负载统计信息，也可以使用性能监视器计数器来跟踪统计信息。  
  
> [!NOTE]  
> 您必须以 Domain Admins 组成员或每台计算机上 Administrators 组的成员身份登录才能完成本主题中所述的任务。 如果你在使用作为 Administrators 组成员的帐户登录时无法完成任务，请尝试在使用域管理员组的成员帐户登录时执行此任务。  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>使用监视仪表板来监视远程访问服务器负载  
  
1.  在“服务器管理器”中，单击“工具”，然后单击“远程访问管理”。  
  
2.  单击“仪表板”以导航到“远程访问管理控制台”中的“远程访问仪表板”。  
  
3.  在监视仪表板上，请注意 "**服务器状态**" 磁贴中的 "**远程客户端状态**" 磁贴。 此磁贴会列出统计信息，例如已连接的远程客户端的总数、已连接的 DirectAccess 客户端的总数和最近24小时内连接的最大用户数。  
  
4.  可以在右窗格中的 "**任务**" 下单击 "**刷新**" 以重新加载运行状况状态。 若要更改默认刷新间隔，请单击 "**任务**" 下的 "**配置刷新间隔**"。  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>使用性能监视器工具监视远程访问服务器上的性能计数器  
  
1.  依次单击 "**开始**"、"**管理工具**" 和 "**性能监视器**"。  
  
2.  在 "**性能**" 下，单击 "**性能监视器**"。  
  
3.  单击 "**性能监视器**" 工具栏中的 "**添加**" 按钮（由绿色十字图标表示）。  
  
4.  从**可用计数器**列表中，选择 " **RAS**和**RAmgmtsvc** " 类别中的所有计数器，然后单击 "**添加 > >** "。  
  
5.  同样，在**可用计数器**列表中，选择 " **IPsec 连接**" 类别中的所有计数器，然后单击 "**添加 > >。**  
  
6.  单击 **"确定"** 以在**性能监视器**控制台中添加所选计数器进行跟踪。  
  
现在，**性能监视器**将以图形方式显示选定的服务器负载统计信息。  
  
@no__t 0Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


