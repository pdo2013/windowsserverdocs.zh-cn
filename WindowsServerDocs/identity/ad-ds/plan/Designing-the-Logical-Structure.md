---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: "设计逻辑结构"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d1b9c27f05faef49f7fd4228f4ebe689b75d30f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-logical-structure"></a>设计逻辑结构

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

Active Directory 域服务(广告 DS)，组织可以创建一个可缩放、安全、更易管理的基础结构的用户和资源管理。 它使它们目录启用应用程序的支持。  
  
场精心设计的 Active Directory 逻辑结构提供以下优势：  
  
-   简化的管理 Microsoft 基于 Windows 的网络，包含大量的对象  
  
-   统一的域结构和管理降低的成本  
  
-   能够委派资源，相应地对管理控制  
  
-   在网络带宽降低的影响  
  
-   简化的资源共享  
  
-   搜索获得最佳性能  
  
-   降低总体拥有成本  
  
场精心设计的 Active Directory 逻辑结构便于组策略; 等功能的高效集成桌面锁定;软件 distribution;为你的系统的用户、组、工作站和服务器管理。 此外，精心设计的逻辑结构便于集成的 Microsoft 和非 Microsoft 应用程序和服务，如 Microsoft Exchange Server，公共基础结构 (PKI)，并且基于域分布式文件系统 (DFS)。  
  
在设计时 Active Directory 逻辑结构部署广告 DS 之前，你可以优化部署过程最佳充分利用 Active Directory 功能。 设计 Active Directory 逻辑结构、设计团队首先标识你的组织的要求，根据该信息、决定放置的森林和域边界的位置。 然后，设计团队决定如何配置为满足的森林域名系统 (DNS) 环境。 最后，设计团队标识所需的资源，在你的组织管理委托部门 (OU) 结构。  
  
## <a name="in-this-guide"></a>本指南中  
  
-   [了解 Active Directory 逻辑模型](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [标识部署项目参与者](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [创建森林设计](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [创建一个域的设计](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [创建 DNS 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [创建单位设计](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [附录 a: DNS 目录获利](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


