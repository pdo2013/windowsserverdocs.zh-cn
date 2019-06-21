---
title: 监视远程访问服务器及其组件的操作状态
description: 本主题是指南的适用于远程访问监视和记帐 Windows Server 2016 中的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9ccfd0cd65a3504349dcad3bd7a549ed18eb6279
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281143"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>监视远程访问服务器及其组件的操作状态

>适用于：Windows 服务器 （半年频道），Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
在远程访问服务器的管理控制台可以用于监视其操作状态。  
  
> [!NOTE]  
> 您必须登录以 Domain Admins 组的成员或每台计算机上 Administrators 组的成员才能完成本主题中介绍的任务。 如果已使用该帐户是 Administrators 组的成员登录时，无法完成任务，请尝试执行该任务，而是 Domain Admins 组的成员帐户登录。  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>若要监视远程访问服务器操作状态  
  
1.  在“服务器管理器”  中，单击“工具”  ，然后单击“远程访问管理”  。  
  
2.  单击**仪表板**以导航到**远程访问报告**中**远程访问管理控制台**。  
  
3.  在监视仪表板，请注意**操作状态**内磁贴**服务器状态**磁贴。 此磁贴列出服务器操作状态和服务器的所有组件的状态。  
  
4.  单击**刷新**下**任务**在右窗格中重新加载操作状态。 操作状态将自动刷新每隔五分钟，这是默认刷新间隔。 若要更改默认刷新间隔，请单击**配置刷新间隔**。  
  
![Windows PowerShell](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
> [!NOTE]  
> 包含引用是适用于群集的操作状态的命令。  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


