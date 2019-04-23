---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: 确定林设计要求
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bf8d9d164bf07151572785cda906be911f97b53e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835328"
---
# <a name="identifying-forest-design-requirements"></a>确定林设计要求

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

若要创建林设计为你的组织，必须确定你的目录结构必须能够容纳的业务要求。 这涉及到决定程度的自治所在的组织需要管理其网络资源和需要隔离网络中的其他组及其资源每个组中的组。  
  
Active Directory 域服务 (AD DS)，您可以设计一个目录基础结构，还包含具有独特管理要求的组织内的多个组，并以实现结构化和操作组之间的独立性根据需要。  
  
你的组织中的组可能具有一些以下类型的要求：  
  
-   **组织结构要求**。 组织的部分可能参与共享的基础结构以节省成本，但需要能够独立于组织的其余部分。 例如，大型组织内的研究组可能需要保持对所有他们自己的研究数据的控制。  
  
-   **操作要求**。 一个组织的一部分可能会施加唯一约束目录服务配置、 可用性或安全性，或者使用应用程序，将它们放在目录上的唯一约束。 例如，在组织内的各业务部门可能会部署已启用目录的修改应用程序目录架构未部署的其他业务部门的。 目录架构在林中所有域之间共享的因为创建多个林是针对这一情况的一种解决方案。 其他示例位于以下组织和方案：  
  
    -   军事组织  
  
    -   托管方案  
  
    -   维护可同时在内部和外部 （例如 Internet 上的用户可公开访问的那些） 的目录的组织  
  
-   **法律要求**。 某些组织具有法律要求进行操作以特定方式，例如，限制对某些信息作为业务协定中指定的访问。 某些组织具有隔离的内部网络上运行的安全要求。 若未满足这些要求可能会导致丢失的协定和操作可能是合法。  
  
确定林设计要求的一部分涉及确定组织中的组可以向其信任潜在林所有者和其服务管理员的程度和确定每个的自治性和隔离要求在你的组织中的组。  
  
设计团队必须文档为打算使用 AD DS 在组织中每个组的服务和数据管理的隔离和自主性要求。 团队还必须记下有限的连接可能会影响部署 AD DS 的任何的区域。  
  
设计团队必须文档为打算使用 AD DS 在组织中每个组的服务和数据管理的隔离和自主性要求。 团队还必须记下有限的连接可能会影响部署 AD DS 的任何的区域。 有关可帮助您记录你标识的区域工作表中，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 从[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)并打开"林设计要求"(DSSLOGI_2.doc)。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [服务管理员的授权范围](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [自主性与。隔离](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
