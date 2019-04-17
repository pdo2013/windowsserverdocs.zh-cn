---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: "设计网站拓扑"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3f16b5b941ef9c3bd8f4bf742d432afc1b3f559a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-site-topology"></a>设计网站拓扑

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

目录服务站点拓扑逻辑代表你的物理网络。 设计 Active Directory 域服务 (广告 DS) 站点拓扑涉及规划域控制器的位置和设计的站点、个子网、站点链接和站点链接桥以确保高效路由查询和复制通信。  
  
设计网站拓扑可帮助你高效发送客户端查询和 Active Directory 复制通信。 场精心设计的网站拓扑可帮助你的组织获得以下好处：  
  
-   最小化复制 Active Directory 数据成本。  
  
-   最小化管理维护站点拓扑只需的工作。  
  
-   安排复制，可使 slow 或拨号网络链接即可复制高峰 Active Directory 数据的位置。  
  
-   优化的客户端计算机查找最接近的资源，如域控制器和分布式文件系统 (DFS) 服务器的能力。 这有助于减少 slow 宽的区域上方的网络交通网络 (WAN) 的链接、改进登录和注销进程和加快文件下载操作。  
  
设计网站拓扑在开始之前，你必须了解你的物理网络结构。 此外，必须首先设计 Active Directory 逻辑结构，包括管理层次、森林套餐，以及每个林域套餐。 您还必须为广告 DS 完成你的域名系统 (DNS) 基础结构设计。 有关设计 Active Directory 逻辑结构和 DNS 基础结构的详细信息，请参阅[设计为 Windows Server 2008 广告 DS 逻辑结构](https://technet.microsoft.com/library/cc770806.aspx)。  
  
完成站点拓扑设计后，你必须验证域控制器满足 Windows Server 2008 Standard、Windows Server 2008 企业版和 Windows Server 2008 Datacenter 的硬件要求。  
  
## <a name="in-this-guide"></a>本指南中  
  
-   [了解 Active Directory 站点拓扑](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [收集网络的信息](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [计划域控制器的位置](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [创建站点设计](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [创建站点链接设计](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [创建网站链接大桥设计](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [查找更多资源设计 Windows Server 2008 Active Directory 站点拓扑](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


