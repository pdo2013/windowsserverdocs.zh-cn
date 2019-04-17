---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: "方案中心访问策略"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1ec4165209b726609b1f9b2caeab02fb5072c756
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-central-access-policy"></a>方案：中心访问策略

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

中心访问文件的策略使组织集中部署和管理授权策略包括条件表情使用用户组、用户索赔，设备索赔和资源属性。 （索赔是对象的声明有关的特性的关联。） 例如，若要访问高业务影响 (HBI) 数据，用户必须全职员工、访问的托管的设备，并与一张智能卡登录。 这些策略定义和托管 Active Directory 域服务 (广告 DS) 中。  
  
组织访问策略驱动合规性和业务法规需求。 例如，如果的组织有业务要求限制对文件所有者和允许查看 PII 信息人工 (HR) 部成员文件中的个人身份信息 (PII) 的访问，此策略适用于 PII 文件随时它们位于文件服务器整个组织。 在此示例中，你需要能够：  
  
-   找出并标记包含 PII 的文件。  
  
-   标识小时成员可以查看 PII 信息的组。  
  
-   创建适用于所有文件包含 PII 随时它们位于文件服务器整个组织中心访问策略。  
  
该计划部署和履行授权策略可以推出由多种原因导致，并对多个级别的组织的应用。 下面是一些示例策略类型：  
  
-   **全组织授权策略。** 最常启动从信息安全 office，此授权策略由符合或高级组织的要求，并且相关整个组织。 例如，都可以访问仅全职员工 HBI 文件。  
  
-   **部门授权的策略。** 每个部门某个组织中的有一些特殊的数据处理要求他们希望强制。 例如，财经商业部可能希望限制访问给财经员工财经服务器。  
  
-   **特定的数据管理策略。** 此策略通常与符合和业务要求，并且它面向保护正确的访问权限管理的信息。 例如，金融机构可能实现信息穿墙或，以便分析不会访问经纪人信息和经纪人不会访问分析的信息。  
  
-   **需要知道的策略。** 此授权策略类型通常配合使用与之前的策略类型。 例如，供应商应该能够访问和编辑仅适用于这些着手的项目的文件。  
  
真实的环境中还学习我们每授权策略需要具有例外，以便重要的企业需要出现时，可以快速反应组织。 例如，官无法找到其智能卡并需要快速访问 HBI 信息可以进行呼叫技术支持部门以获取临时例外，若要访问此类信息。  
  
中央访问策略作为组织适用于其服务器的安全 umbrellas。 这些策略增强（但不要替换）本地访问策略或随机访问控件列表 (DACL) 应用的文件和文件夹。 例如，如果在某个文件上的 DACL 允许访问特定用户，但应用于文件中央策略限制相同用户访问，用户无法获得对该文件的访问权限。 如果中心访问策略允许访问权限，但 DACL 不允许访问，用户将无法获得对该文件的访问权限。  
  
中心访问策略规则具有以下几个逻辑部分：  
  
-   **适用性。** 条件定义哪些数据策略适用于，如 Resource.BusinessImpact=High。  
  
-   **访问条件。** 项的列表一个或多个访问控制 (Ace) 定义人可以访问该数据，如允许 |完全控制 |User.EmployeeType=FTE。  
  
-   **例外情况。** 定义策略，如 MemberOf(HBIExceptionGroup) 异常的一个或多个 Ace 其他列表。  
  
下面的两个图显示在中心访问工作流，审核策略。  
  
![解决方案指南](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**图 1**中央访问和审核策略概念  
  
![解决方案指南](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**图 2**中心访问策略工作流  
  
中央授权策略组合以下组件：  
  
-   集中定义的访问规则针对特定类型的信息，如 HBI 或 PII 的列表。  
  
-   集中定义的策略包含规则的列表。  
  
-   分配到文件服务器上的每个文件指向应期间访问授权应用特定中心访问策略策略标识符。  
  
下图说明了如何将策略合并到策略集中控制访问文件的列表。  
  
![解决方案指南](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**图 3**组合策略  
  
## <a name="in-this-scenario"></a>在此情况下  
以下指南是会向你中心访问策略提供：  
  
-   [计划中央访问策略部署](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [部署中心访问策略 #40; 演示步骤 & #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>负责并此方案中所含功能  
下表列出的角色和属于这种情况下的功能，并介绍了如何支持。  
  
|角色/功能|它如何支持此方案|  
|-----------------|---------------------------------|  
|Active Directory 域服务角色|在 Windows Server 2012 的广告 DS 引入了支持用户索赔和设备索赔、复合身份、（用户以及设备索赔），创建一个索赔基于授权平台新中心访问策略（笔帽）的模型，并在授权决定文件分类信息的使用。|  
|文件和存储服务服务器角色|文件和存储服务提供帮助您的技术设置并管理你的网络，你可以在此处存储文件并将它们与用户共享提供中心位置的一个或多个文件服务器。 如果您的网络的用户需要访问相同的文件和应用程序，或者备份和文件的集中的管理很重要，对你的组织，你应该设置一个或多个计算机文件服务器，在作为通过向计算机添加的文件和存储服务的角色和相应角色服务。|  
|Windows 客户端计算机|用户可以通过客户端计算机访问的文件和网络上的文件夹。|  
  


