---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: 设计站点拓扑
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e1d9323ceda478369973f959687d46c9ca3cb88f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888488"
---
# <a name="designing-the-site-topology"></a>设计站点拓扑

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

目录服务站点拓扑是在物理网络的逻辑表示形式。 为 Active Directory 域服务 (AD DS) 设计站点拓扑需要规划域控制器放置和设计站点、 子网、 站点链接和站点链接桥，以便确保有效的查询和复制流量的路由。  
  
设计站点拓扑，可帮助你有效地将客户端查询和 Active Directory 复制流量的路由。 设计良好的站点拓扑可帮助组织实现以下优势：  
  
-   将复制 Active Directory 数据的成本降至最低。  
  
-   最大程度减少维护站点拓扑所需的管理工作量。  
  
-   计划复制，其中包含用于在非高峰时间复制 Active Directory 数据的慢速或拨号网络链接的位置。  
  
-   优化客户端计算机能够找到的最接近的资源，例如域控制器和分布式文件系统 (DFS) 服务器。 可帮助减少网络流量通过慢速广域网网络 (WAN) 链接、 改进登录和注销过程，并提高文件下载操作的速度。  
  
在开始设计站点拓扑之前，必须了解您的物理网络的结构。 此外，您必须首先设计 Active Directory 逻辑结构，包括管理层次结构、 林计划和每个林的域计划。 适用于 AD DS，你还必须完成你的域名系统 (DNS) 基础结构设计。 有关设计 Active Directory 逻辑结构和 DNS 基础结构的详细信息，请参阅[设计逻辑结构的 Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)。  
  
完成您的站点拓扑设计后，您必须验证你的域控制器，满足 Windows Server 2008 Standard、 Windows Server 2008 Enterprise 和 Windows Server 2008 Datacenter 的硬件要求。  
  
## <a name="in-this-guide"></a>本指南包含的内容  
  
-   [了解 Active Directory 站点拓扑](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [收集网络信息](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [规划域控制器放置](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [创建站点设计](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [创建站点链接设计](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [创建站点链接桥设计](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [为 Windows Server 2008 Active Directory 站点拓扑设计查找其他资源](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


