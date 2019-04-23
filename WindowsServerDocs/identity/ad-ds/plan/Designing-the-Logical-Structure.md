---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: 设计逻辑结构
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 82184fe678c05b1d7458584de8eecd0c07ece02f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832328"
---
# <a name="designing-the-logical-structure"></a>设计逻辑结构

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 域服务 (AD DS) 使组织能够创建用于用户和资源管理的可伸缩、 安全及可管理基础结构。 它还使他们能够支持启用目录的应用程序。  
  
设计良好的 Active Directory 逻辑结构提供了以下优势：  
  
-   Microsoft 基于 Windows 的网络包含大量对象的简化的管理  
  
-   合并的域结构和降低的管理成本  
  
-   委派管理控制资源，根据需要的功能  
  
-   减少对网络带宽的影响  
  
-   简化的资源共享  
  
-   最佳搜索性能  
  
-   较低总拥有成本  
  
设计良好的 Active Directory 逻辑结构有助于高效为组策略; 此类功能的集成桌面锁定;软件分发;和到您的系统的用户、 组、 工作站和服务器管理。 此外，精心设计的逻辑结构促进 Microsoft 和非 Microsoft 应用程序和服务，如 Microsoft Exchange Server，公钥基础结构 (PKI) 的集成，并基于域的分布式文件系统 (DFS)。  
  
在部署 AD DS 之前，Active Directory 逻辑结构设计时，可以优化部署过程以最有效地利用 Active Directory 功能。 若要设计 Active Directory 逻辑结构，您的设计团队首先标识为你的组织的要求和，根据此信息，在决定的林和域边界的放置位置。 然后，设计团队决定如何配置域名系统 (DNS) 环境，以满足在林中的需求。 最后，设计团队标识你的组织中的资源的管理委派所需的组织单位 (OU) 结构。  
  
## <a name="in-this-guide"></a>本指南包含的内容  
  
-   [了解 Active Directory 逻辑模型](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [确定部署项目参与者](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [创建林设计](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [创建域设计](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [创建 DNS 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [创建组织单位设计](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [附录 a:DNS 清单](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


