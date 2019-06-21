---
title: 使用历史数据为远程客户端生成使用情况报告
description: 本主题是指南的适用于远程访问监视和记帐 Windows Server 2016 中的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cfbac18f64123f97c54b29c1aeef7364af55e49a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281124"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>使用历史数据为远程客户端生成使用情况报告

>适用于：Windows 服务器 （半年频道），Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
远程访问服务器上的管理控制台可用于为在访问服务器的远程客户端生成使用情况报告。 若要为远程客户端生成使用情况报告，第一次启用远程访问服务器上的记帐。 生成报告之后，可以使用可在管理控制台中远程访问服务器上要在服务器上查看负载统计信息在监视仪表板。  
  
> [!NOTE]  
> 您必须登录以 Domain Admins 组的成员或每台计算机上 Administrators 组的成员才能完成本主题中所述的任务。 如果已使用该帐户是 Administrators 组的成员登录时，无法完成任务，请尝试执行该任务，而是 Domain Admins 组的成员帐户登录。  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>若要启用远程访问服务器上的记帐  
  
1.  在“服务器管理器”  中，单击“工具”  ，然后单击“远程访问管理”  。  
  
2.  单击**报告**以导航到**远程访问报告**中**远程访问管理控制台**。  
  
3.  单击**配置记帐**中**远程访问报告**任务窗格。  
  
4.  选择**使用收件箱记帐**复选框以启用远程访问服务器上的记帐。  
  
5.  单击**Apply**以启用记帐配置的服务器上，然后单击**关闭**服务器已成功应用配置后。  
  
#### <a name="to-generate-the-usage-report"></a>若要生成使用情况报告  
  
1.  在“服务器管理器”  中，单击“工具”  ，然后单击“远程访问管理”  。  
  
2.  单击**报告**以导航到**远程访问报告**中**远程访问管理控制台**。  
  
3.  在中间窗格中，单击日历中的日期，以选择在报表持续**开始日期：** 并**结束日期：** ，然后单击**生成报告**。  
  
4.  您将看到在所选的时间和有关其详细统计信息中已连接到远程访问服务器的用户的列表。 单击列表中的第一行。 当选中某行时，在预览窗格中显示的远程用户活动。 现在，选择**服务器加载统计信息**在预览窗格中，若要查看历史负载的服务器上的选项卡。  
  
    单击**服务器加载统计信息**在预览窗格中，若要查看历史负载的服务器上的选项卡。  
  
> [!NOTE]  
> **了解会话**  
>   
> 远程访问核算基于的概念**会话**。 与此相反**连接**即**会话**由远程客户端 IP 地址和用户名称的组合唯一标识。 例如，在机器隧道形式从远程客户端，名为 Client1，则将创建一个会话，并存储在记帐数据库。 时将传递一段时间后，来自该客户端连接名为的 User1 的用户 （但在机器隧道仍处于活动状态），该会话记录为单独的会话。 会话的区别是保留在机器隧道和用户隧道之间的区别。  
  
![Windows PowerShell](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
在以下脚本中，更改要在生成报表的日期范围 **-StartDateTime**并 **-EndDateTime**参数。  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


