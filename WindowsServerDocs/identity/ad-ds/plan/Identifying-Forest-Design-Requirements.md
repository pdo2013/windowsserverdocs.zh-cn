---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: 确定林设计要求
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 41caeca82819eaea3d86d5f1eb4883ab8bbf53cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408790"
---
# <a name="identifying-forest-design-requirements"></a>确定林设计要求

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要为你的组织创建林设计，你必须确定你的目录结构需要满足的业务要求。 这涉及到确定组织中的组管理其网络资源所需的独立性量，以及每个组是否需要将其资源从其他组隔离在网络上。  
  
Active Directory 域服务（AD DS）可用于设计一个目录基础结构，该基础结构可容纳组织内具有独特管理要求的多个组，并实现组间的结构化和操作独立性根据需要。  
  
你的组织中的组可能具有以下类型的要求：  
  
-   **组织结构要求**。 组织的各个部分可能会参与共享的基础结构，以节省成本，但需要能够独立于组织的其余部分进行操作。 例如，大型组织内的研究组可能需要保持对其所有数据的控制。  
  
-   **操作要求**。 组织的一个部分可能对目录服务配置、可用性或安全性施加唯一约束，或者使用在目录中放置唯一约束的应用程序。 例如，组织内的各个业务部门可能会部署已启用目录的应用程序，这些应用程序修改了其他业务部门未部署的目录架构。 由于目录架构是在林中所有域之间共享的，因此创建多个林是一种适用于这种情况的解决方案。 以下组织和方案中可找到其他示例：  
  
    -   军事组织  
  
    -   宿主方案  
  
    -   维护可在内部和外部使用的目录的组织（例如，Internet 上的用户可公开访问的目录）  
  
-   **法律要求**。 某些组织的法律要求是以特定的方式运行的，例如，将访问权限限制为业务协定中指定的某些信息。 某些组织对隔离的内部网络具有安全要求。 如果无法满足这些要求，可能会导致合同丢失，并且可能会导致法律诉讼。  
  
确定林设计要求的一部分涉及到识别组织中的组可信任潜在林所有者及其服务管理员，并确定每个组的独立性和隔离要求的程度组织中的组。  
  
设计团队必须记录组织中打算使用 AD DS 的每个组的隔离和自治要求。 团队还必须注意可能会影响 AD DS 部署的有限连接性区域。  
  
设计团队必须记录组织中打算使用 AD DS 的每个组的隔离和自治要求。 团队还必须注意可能会影响 AD DS 部署的有限连接性区域。 要使工作表可以帮助您记录所标识的区域，请从[Windows Server 2003 部署工具包的作业助手](https://go.microsoft.com/fwlink/?LinkID=102558)下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开 "林设计要求" （DSSLOGI_2）。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [服务管理员的授权范围](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [自主性与隔离](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
