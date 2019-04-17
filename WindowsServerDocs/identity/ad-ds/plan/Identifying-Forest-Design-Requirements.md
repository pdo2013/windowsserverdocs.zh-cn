---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: "确定森林设计要求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 91d551e5d7b6daa6b1476570dad57e85d3bc163f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-forest-design-requirements"></a>确定森林设计要求

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

若要创建你的组织的森林设计，你必须识别目录结构需要容纳的业务要求。 这包括确定多少自主组织需管理其网络资源，并且需要隔离他们从其他组网络上的资源每个组中的组。  
  
Active Directory 域服务 (广告 DS) 使您能够设计目录基础结构，可使用在企业中有很多唯一管理要求的多个组中，并实现结构和运营独立组根据需要之间。  
  
你的组织中的组可能有一些以下类型的要求：  
  
-   **组织结构要求**。 部分的组织可能参与共享的基础结构，以保存成本，但需要能够独立从组织中的其余操作。 例如，在较大的组织中研究组可能需要保持对所有自己研究数据控制。  
  
-   **运行要求**。 组织的一个组成部分可能又唯一限制目录服务配置、 可用性或安全，或使用唯一约束目录的位置的应用程序。 例如，组织内的个别企业单位可能部署目录启用应用程序修改目录方案，不会通过其他业务部门部署。 森林中的所有域之间共享目录方案，因为创建多个林是这种情况下的一个解决方案。 在以下组织和方案中找到的其他示例：  
  
    -   军事组织  
  
    -   举办方案  
  
    -   维护可以目录内部和外部 （如这些是可通过 Internet 上的用户公开访问） 的组织  
  
-   **法律要求**。 某些组织具有法律的要求进行操作以特定的方式，例如，限制对规定业务合同某些信息的访问。 某些组织提供的安全要求，以独立内部网络上进行操作。 未能满足这些需求可能导致丢失合同和可能法律操作。  
  
确定森林设计需求的一部分涉及识别的度数潜在的森林所有者和它们的服务管理员你的组织中的组信任的并确定你的组织中的每组自主和隔离要求。  
  
设计团队必须文档对手每组打算使用广告 DS 组织中的服务和数据管理隔离和独立要求。 团队还请注意受限的连接可能会影响广告 DS 部署的任何领域。  
  
设计团队必须文档对手每组打算使用广告 DS 组织中的服务和数据管理隔离和独立要求。 团队还请注意受限的连接可能会影响广告 DS 部署的任何领域。 为了帮助您中记录你标识地区工作表中，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip)。  
  
## <a name="in-this-section"></a>在此部分中  
  
-   [颁发机构的服务管理员范围](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [自主与隔离](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
  


