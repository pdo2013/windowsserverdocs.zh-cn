---
title: 监视用于活动和状态的连接的远程客户端
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
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a38e5682b03cdb37ff88332317122b6addd000c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446876"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>监视用于活动和状态的连接的远程客户端

>适用于：Windows 服务器 （半年频道），Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 和远程访问服务 (RAS) 合并到单个远程访问角色。  
  
可以使用远程访问服务器上的管理控制台来监视远程客户端活动和状态。  
  
> [!NOTE]  
> 您必须登录以 Domain Admins 组的成员或每台计算机上 Administrators 组的成员才能完成本主题中所述的任务。 如果已使用该帐户是 Administrators 组的成员登录时，无法完成任务，请尝试执行该任务，而是 Domain Admins 组的成员帐户登录。  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>若要监视远程客户端活动和状态  
  
1.  在“服务器管理器”  中，单击“工具”  ，然后单击“远程访问管理”  。  
  
2.  单击**报告**以导航到**远程访问报告**中**远程访问管理控制台**。  
  
3.  单击**远程客户端状态**导航到的远程客户端活动和状态用户界面中**远程访问管理控制台**。  
  
4.  您将看到的用户连接到远程访问服务器，并详细的列表有关它们的统计信息。 单击对应于客户端的列表中的第一行。 当选中某行时，在预览窗格中显示的远程用户活动。  
  
![Windows PowerShell](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
可以筛选用户统计信息，根据所选的条件，通过使用下表中的字段。  
  
|字段名称|值|  
|-------|-----|  
|Username|远程用户的用户名或别名。 可以使用通配符选择一组用户，如 contoso\\* 或\*\administrator。|  
|主机名|远程用户的计算机帐户名称。 此外可以指定 IPv4 或 IPv6 地址。|  
|在任务栏的搜索框中键入|DirectAccess 或 VPN。 如果选择了 DirectAccess，列出所有远程用户使用 DirectAccess 连接。 如果选择了 VPN，列出所有远程用户通过 VPN 连接。|  
|ISP 地址|远程用户的 IPv4 或 IPv6 地址。|  
|IPv4 地址|将远程用户连接到公司网络隧道的内部 IPv4 地址。|  
|IPv6 地址|将远程用户连接到企业网络的隧道的内部 IPv6 地址。|  
|协议/隧道|远程客户端使用转换技术。 这对于 DirectAccess 用户，是 Teredo、 6to4 或 IP-HTTPS，它对 VPN 用户是 PPTP、 L2TP，SSTP 或 IKEv2。|  
|访问的资源|所有访问特定企业资源或终结点的用户。 对应于此字段的值是服务器的主机名 /IP 地址。|  
|Server|客户端所连接到的远程访问服务器。 这仅与群集和多站点部署相关。|  
  
  
  


