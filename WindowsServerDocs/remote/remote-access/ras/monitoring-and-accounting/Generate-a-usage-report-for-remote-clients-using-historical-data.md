---
title: 使用历史数据为远程客户端生成使用情况报告
description: 本主题是 Windows Server 2016 中的远程访问监视和记帐指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bae50345e8a6fd4018857e2a754d0274ce02855d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367250"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>使用历史数据为远程客户端生成使用情况报告

>适用于：Windows Server（半年频道）、Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
远程访问服务器上的管理控制台可用于为访问服务器的远程客户端生成使用情况报告。 若要为远程客户端生成使用情况报告，请首先在远程访问服务器上启用记帐。 生成报告后，可以使用远程访问服务器上的管理控制台中提供的监视仪表板来查看服务器上的负载统计信息。  
  
> [!NOTE]  
> 您必须以 Domain Admins 组成员或每台计算机上 Administrators 组的成员身份登录才能完成本主题中所述的任务。 如果你在使用作为 Administrators 组成员的帐户登录时无法完成任务，请尝试在使用域管理员组的成员帐户登录时执行此任务。  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>在远程访问服务器上启用记帐  
  
1.  在“服务器管理器”中，单击“工具”，然后单击“远程访问管理”。  
  
2.  单击 "**报告**"，导航到**远程访问管理控制台**中的**远程访问报告**。  
  
3.  单击 "**远程访问报告**" 任务窗格中的 "**配置记帐**"。  
  
4.  选中 "**使用收件箱记帐**" 复选框以在远程访问服务器上启用记帐。  
  
5.  单击 "**应用**" 以在服务器上启用记帐配置，然后在服务器成功应用配置后单击 "**关闭**"。  
  
#### <a name="to-generate-the-usage-report"></a>生成使用情况报告  
  
1.  在“服务器管理器”中，单击“工具”，然后单击“远程访问管理”。  
  
2.  单击 "**报告**"，导航到**远程访问管理控制台**中的**远程访问报告**。  
  
3.  在中间窗格中，单击日历中的 "日期" 以选择报表持续时间的**开始日期：** 和**结束日期：** ，然后单击 "**生成报表**"。  
  
4.  你将看到在所选时间内连接到远程访问服务器的用户的列表以及有关这些用户的详细统计信息。 单击列表中的第一行。 选择行时，"远程用户" 活动将显示在预览窗格中。 现在，在 "预览" 窗格中选择 "**服务器负载统计信息**" 选项卡，以查看服务器上的历史负载。  
  
    单击预览窗格中的 "**服务器负载统计信息**" 选项卡，以查看服务器上的历史负载。  
  
> [!NOTE]  
> **了解会话**  
>   
> 远程访问记帐基于**会话**的概念。 与**连接**相比，**会话**是由远程客户端 IP 地址和用户名的组合唯一标识的。 例如，如果从名为 Client1 的远程客户端形成计算机隧道，则将创建一个会话并将其存储在记帐数据库中。 当某个名为 User1 的用户在经过一段时间后从该客户端进行连接（但计算机隧道仍处于活动状态）时，该会话将记录为一个单独的会话。 会话的区别在于在计算机隧道和用户隧道之间保留差异。  
  
@no__t 0Windows PowerShell](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
在下面的脚本中，在 **-StartDateTime**和 **-EndDateTime**参数中更改需要报表的日期范围。  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


