---
title: 远程管理 DirectAccess 客户端
description: 本主题是在 Windows Server 2016 中远程管理 DirectAccess 客户端的指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36255d80-a13e-4af7-a5c0-ab4c8f302622
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecfbd6dc09f251d5197a16b166aa76e6a37b933a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367277"
---
# <a name="manage-directaccess-clients-remotely"></a>远程管理 DirectAccess 客户端

>适用于：Windows Server（半年频道）、Windows Server 2016

远程访问监视报告远程用户活动以及 DirectAccess 和 VPN 连接的状态。 它跟踪客户端连接的数量和持续时间（以及其他的统计信息），并监视服务器的操作状态。 易于使用的监视控制台允许你查看整个远程访问基础设施。 监视视图可用于单台服务器、群集和多站点配置。  
  
**注意：** Windows Server 2016 将 DirectAccess 和远程访问服务（RAS）合并到单个远程访问角色中。  
  
## <a name="in-this-guide"></a>本指南包含的内容  
本文档说明如何使用 DirectAccess 管理控制台和相应 Windows PowerShell cmdlet（作为远程访问服务器角色的一部分提供）来利用远程访问的监视功能。  
  
将对下面的监视和记帐方案进行说明：  
  
1.  监视远程访问服务器上的现有负载  
  
2.  监视远程访问服务器的配置分发状态  
  
3.  监视远程访问服务器及其组件的操作状态  
  
4.  识别并解决远程访问服务器操作问题  
  
5.  监视用于活动和状态的连接的远程客户端  
  
6.  使用历史记录数据生成远程客户端的使用情况报告  
  
### <a name="understand-monitoring-and-accounting"></a>了解监视和记帐  
为远程客户端开始监视和记帐任务之前，你需要了解二者的区别。  
  
-   **监视**显示在给定时间点有效连接的用户。  
  
-   **记帐**保留已连接到企业网络的用户历史记录及其使用情况详细信息（用于合规性和审核）。  
  
远程客户端监视基于连接。 存在两种类型的由 DirectAccess 客户端建立的隧道连接：  
  
-   **计算机隧道流量连接**：在系统上下文中，该隧道将由计算机建立以访问名称解析、身份验证、修正更新等所需的服务器。  
  
-   **用户隧道流量连接**：在用户上下文中，当用户尝试访问企业网络上的资源时，该隧道将由计算机上的用户帐户建立。 根据部署要求，用户可能需要提供强凭据（例如，通过使用智能卡或提供一次性密码）来访问企业网络资源。  
  
对于 DirectAccess，连接由远程客户端的 IP 地址进行唯一标识。 例如，如果为客户端计算机打开计算机隧道，并且某位用户从该计算机连接，这将会使用相同的连接。 如果在机器隧道仍处于活动状态的同时，用户断开连接并再次连接，这是一个单一连接。  
  


