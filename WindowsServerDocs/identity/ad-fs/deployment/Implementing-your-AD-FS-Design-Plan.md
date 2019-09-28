---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: 实现 AD FS 设计规划
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6306b87dd06774bfde5ffc3ff98818d47d0c858f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408389"
---
# <a name="implementing-your-ad-fs-design-plan"></a>实现 AD FS 设计规划

以下环境条件和要求是实现 Active Directory 联合身份验证服务 \(AD FS @ no__t 设计计划的重要因素：  
  
-   **支持的合作伙伴：** 通常使用 AD FS 与合作伙伴组织合作。 若要建立联合身份验证，请确定要建立合作关系的组织。 完成基准 AD FS 部署后，与合作伙伴合作涉及到添加合作伙伴、删除合作伙伴以及更新合作伙伴信息。 由于各种原因，可能会发生对合作关系的更改。 例如 AD FS，如果你的合作伙伴重大更改其业务，你的组织将成为大型组织或组织联合的一部分，或者你的组织由不同的上市公司. 在你联合多个域中的标识的任何方案中，你都需要知道当前支持的域 \(partners @ no__t-1，以及表示潜在合作伙伴的所有其他域。  
  
-   **支持的应用程序和服务类型：** 某些应用程序和服务需要访问操作系统资源，而其他应用程序和服务则为 "声明感知"。 务必要了解 AD FS 支持的应用程序和服务的类型，以便能够制定管理要求。  
  
-   **逻辑和物理体系结构关系图或部署拓扑：** 你将需要知道：  
  
    -   联合服务器是在一组场服务器还是单个服务器上运行。  
  
    -   网络部署防火墙和代理的位置。  
  
    -   资源的位置以及用户是从组织内部还是在组织外部访问资源。  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>如何使用本指南实现 AD FS 设计  
实现设计的下一步是确定每个部署任务必须执行的顺序。 本指南使用清单来帮助你逐步完成实现设计规划所需的各种服务器和应用程序部署任务。 根据需要使用父和子清单来表示特定 AD FS 设计的任务必须处理的顺序。  
  
使用本指南的此部分中的以下父清单来熟悉用于实现组织的首选 AD FS 设计的部署任务：  
  
-   [清单：实现 Web SSO 设计](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [清单：实现联合 Web SSO 设计](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
