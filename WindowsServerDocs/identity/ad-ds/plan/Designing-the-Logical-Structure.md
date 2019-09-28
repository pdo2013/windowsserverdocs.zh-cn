---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: 设计逻辑结构
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8d72d7ed9617d18b42f1be10daeafbac994dad88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402632"
---
# <a name="designing-the-logical-structure"></a>设计逻辑结构

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 域服务（AD DS）使组织能够为用户和资源管理创建可缩放的、安全且可管理的基础结构。 它还使它们能够支持启用目录的应用程序。  
  
设计良好的 Active Directory 逻辑结构具有以下优势：  
  
-   简化了包含大量对象的基于 Microsoft Windows 的网络的管理  
  
-   合并域结构并降低管理成本  
  
-   根据需要委派对资源的管理控制  
  
-   降低对网络带宽的影响  
  
-   简化资源共享  
  
-   最佳搜索性能  
  
-   总拥有成本较低  
  
设计良好的 Active Directory 逻辑结构有助于有效地将此类功能与组策略集成;桌面锁定;软件分发;以及用户、组、工作站和服务器管理。 此外，经过精心设计的逻辑结构有助于 Microsoft 和非 Microsoft 应用程序和服务（如 Microsoft Exchange Server、公钥基础结构（PKI）和基于域的分布式文件系统（DFS））的集成。  
  
在部署 AD DS 之前设计 Active Directory 逻辑结构时，你可以优化部署过程以充分利用 Active Directory 功能。 为了设计 Active Directory 逻辑结构，你的设计团队首先确定你的组织的要求，并且根据此信息确定放置林和域边界的位置。 然后，设计团队决定如何配置域名系统（DNS）环境以满足林的需求。 最后，设计团队确定委托组织中资源管理所需的组织单位（OU）结构。  
  
## <a name="in-this-guide"></a>本指南包含的内容  
  
-   [了解 Active Directory 逻辑模型](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [确定部署项目参与者](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [创建林设计](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [创建域设计](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [创建 DNS 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [创建组织单位设计](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [附录 A：DNS 清单](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


