---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: "方案文件访问审核"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 93d78bbefce38173198f991543fb3a06d145b373
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-file-access-auditing"></a>方案：访问审核文件

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

安全审核是一款功能最强大的工具，以帮助维持企业级的安全性。 关键的安全审核的目标之一是合规性。 如 Sarbanes-oxley、医疗保险责任法案 (HIPAA) 和支付卡 Industry (PCI) 行业标准要求遵守严格的一组规则与相关的数据安全和隐私的企业。 安全审核帮助建立的此类策略存在和证明遵守这些标准。 此外，安全审核帮助检测到异常行为，识别，并减少安全策略中的缺陷，并通过创建用户活动的用于分析鉴定古道阻止可靠的行为。  
  
在以下级别通常驱动审核策略要求：  
  
-   **信息安全。** 文件的访问权限审核常用鉴定的分析和入侵检测。 能够让我们来获取对高值信息的访问有关有针对性的事件企业相当提高其响应时间和调查准确性。  
  
-   **组织的策略。** 例如，组织受 PCI 标准可能能够标记为包含个人身份信息 (PII) 和信用卡信息的所有文件中央策略监视器访问。  
  
-   **部门策略。** 例如，能够修改（如的季度收入报告）某些财经文档将限制为财经部门，并因此部门想要监视器所有其他尝试更改这些文档，则可能需要财经部门。  
  
-   **企业策略。** 例如，商家可能需要监视器所有未经授权的尝试以查看与他们的项目所属的数据。  
  
此外，合规性商业部可能需要监视器中心授权的策略和如用户、计算机和资源属性策略结构的所有更改。  
  
安全审核大事项之一就是收集、存储和分析审核事件的费用。 如果审核策略范围太大，耸立在审核收集的事件的音量，并这会增加成本。 如果审核策略太窄时，你可能丢失重要的事件。  
  
与 Windows Server 2012，你可以通过使用索赔和资源属性创作审核策略。 这导致了更丰富，更有针对性的更容易管理审核策略。 它使方案，直到现在，已无法或难以来执行。 管理员可以编写的审核策略的示例如下：  
  
-   审核每个人都不具有高安全许可并尝试访问 HBI 文档。 例如，审核 |每个人都 |全部访问 |Resource.BusinessImpact=HBI 并 User.SecurityClearance!=High。  
  
-   在尝试访问不使用的项目相关的文档时审核所有供应商。 例如，审核 |每个人都 |全部访问 |User.EmploymentStatus=Vendor 并 User.Project Not_AnyOf Resource.Project。  
  
这些策略帮助规定审核事件的音量，并限制他们最相关的数据或用户。  
  
创建和应用的审核策略管理员后下, 一步注意事项 gleaning 从其收集审核事件有意义的信息。 基于 expression 审核事件帮助减少审核的音量。 但是，用户需要一种方法查询这些事件有意义的信息，如提问"会访问我 HBI 数据？" 或者"没有程度未经授权的尝试访问敏感数据？"  
  
 Windows Server 2012 增强了与用户、计算机和资源的索赔现有数据的访问权限事件。 这些事件在每个服务器基础上生成。 整个组织提供的事件全屏视图中，Microsoft 将与合作伙伴合作，以提供事件集锦和分析工具，例如审核集锦服务中 System Center 操作 Manager 合作。  
  
图 4 显示中央审核策略的概述。  
  
![解决方案指南](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**图 4**中央审核体验  
  
设置和通常消耗安全审核涉及以下常规步骤：  
  
1.  找出组正确的数据和用户监视器  
  
2.  创建和应用相应审核策略  
  
3.  收集和分析审核事件  
  
4.  管理和监视器创建的策略  
  
## <a name="in-this-scenario"></a>在此情况下  
以下主题提供此项 scenario 其他的指南：  
  
-   [文件计划访问审核](Plan-for-File-Access-Auditing.md)  
  
-   [部署安全审核与中心审核策略 #40; 演示步骤 & #41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>负责并此方案中所含功能  
下表列出的角色和属于这种情况下的功能，并介绍了如何支持。  
  
|角色/功能|它如何支持此方案|  
|-----------------|---------------------------------|  
|Active Directory 域服务角色|在 Windows Server 2012 的广告 DS 引入了基于索赔授权平台，可使用户索赔和设备索赔、复合身份、（用户以及设备索赔），创建新中心访问策略（笔帽）型号和文件分类信息，在授权决定使用。|  
|文件和存储服务的作用|文件服务器，在 Windows Server 2012 提供用户界面管理员查看有效权限的用户配置文件或文件夹和疑难解答访问问题和授予根据需要的访问权限。|  
  


