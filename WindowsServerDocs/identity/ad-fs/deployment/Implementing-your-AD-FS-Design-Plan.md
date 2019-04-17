---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: "实现广告 FS 设计套餐"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 31c2e048c3c125d0ea60610b049501151d7aa823
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="implementing-your-ad-fs-design-plan"></a>实现广告 FS 设计套餐

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

环境的以下条件和需求是在你的 Active Directory 联合身份验证服务 \(AD FS\) 设计的套餐实现重要因素：  
  
-   **受支持的合作伙伴：**你通常使用广告 FS 处理合作伙伴组织。 若要建立联合身份验证，确定你想要形成合作的组织。 基准广告 FS 部署准备好后，保持与合作伙伴涉及合作伙伴添加、删除合作伙伴和更新合作伙伴的信息。 对合作的更改可能会出现的各种原因。 例如，广告 FS 部署可能需要合作更新，如果您的合作伙伴更改其业务显著、你的组织变得更大组织或组织机构的联合身份验证的一部分，或由其他公司获取你的组织。 在任何从多个域的身份联合在其中的情况下，你将需要知道您当前在支持域 \(partners\) 和所有其他域表示潜在的合作伙伴。  
  
-   **受支持应用程序和服务类型：**某些应用程序和服务需要访问操作系统资源，而其他"索赔注意。" 请务必了解类型的应用程序和服务广告 FS 支持，以便你可以制定管理需求。  
  
-   **逻辑和物理体系结构图表或部署拓扑：**你将需要知道：  
  
    -   是否在场服务器或一台服务器上的一组中的工作联合身份验证的服务器。  
  
    -   在你的网络部署防火墙和代理服务器。  
  
    -   资源，以及用户是否访问从外部和 / 或组织机构，你的组织中的资源的位置。  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>如何实现广告 FS 设计使用本指南  
下一步中实现你设计是确定顺序必须执行每个部署任务。 本指南使用清单以帮助你逐步完成的各种服务器和应用程序部署任务所需以实现你设计的套餐。 使用家长和孩子清单为必要前提代表必须处理特定的广告 fs 哪些任务设计的顺序。  
  
在此部分中的指南使用下列父清单熟悉的实现你的组织的首选的广告 FS 设计部署任务：  
  
-   [清单：实施 Web SSO 设计](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [清单：实施联盟的 Web SSO 设计](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
