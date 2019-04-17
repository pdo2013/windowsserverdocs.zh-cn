---
ms.assetid: 44271f44-b50a-4bce-9375-4fcab9618048
title: "清单-依赖方创建索赔规则信任"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9c75cd4ccbafefdda83cba4551fd6b9af63c4822
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-creating-claim-rules-for-a-relying-party-trust"></a>清单：创建适用于信赖的方信任索赔规则

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本文包含所必需的计划、设计和部署的 Active Directory 联合身份验证服务 \(AD FS\) 信赖的方信任与相关联的索赔规则的任务。  
  
> [!NOTE]  
> 完成此清单订单中的任务。 当参考链接将你带到过程时，返回到本主题之后你完成该过程中的步骤，以便你可以继续进行此清单中的其余任务。  
  
![创建声称规则](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：创建索赔规则信赖的方信任的设置**  
  
||任务|参考|  
|-|--------|-------------|  
|![创建索赔规则](media/icon_checkboxo.gif)|了解有关索赔概念、声称规则、声称规则集和声称规则模板，以及如何与联盟信任关联。|![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[索赔角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)<br /><br />![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[声称规则的作用](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)|  
|![创建索赔规则](media/icon_checkboxo.gif)|了解有关通过索赔颁发管道中的所有阶段索赔的排列方式和索赔颁发引擎如何处理规则的概念。|![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[的索赔管道的作用](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)<br /><br />![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[角色的索赔引擎](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)|  
|![创建索赔规则](media/icon_checkboxo.gif)|实际上规划和实现将通过这种信赖方信任发布输出索赔，确定是否需要安装一个或多个索赔规则，并且其中声称的规则，你应使用与此依赖方信任。|![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定类型的声称规则模板到使用](../../ad-fs/technical-reference/Determine-the-Type-of-Claim-Rule-Template-to-Use.md)|  
|![创建索赔规则](media/icon_checkboxo.gif)|了解有关概念时创建一个声明规则通过另一个和如何使用索赔规则语言中提供比标准规则更复杂逻辑，以便提供所需的理想的输出结果声称设置。|![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用穿透或筛选声称规则](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)<br /><br />![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用转换声称规则](../../ad-fs/technical-reference/When-to-Use-a-Transform-Claim-Rule.md)<br /><br />![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用作为索赔规则发送 LDAP 属性](../../ad-fs/technical-reference/When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)<br /><br />![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时声称通常使用发送组成员资格](../../ad-fs/technical-reference/When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)<br /><br />![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用授权声称规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)<br /><br />![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用自定义声称规则](../../ad-fs/technical-reference/When-to-Use-a-Custom-Claim-Rule.md)<br /><br />![创建声称规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[声称规则语言的作用](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)|  
|![创建索赔规则](media/icon_checkboxo.gif)|如果不存在，则必须创建索赔描述，将履行你的组织的需求。 广告 FS 附带了一组默认的索赔说明中 snap\ 中广告 FS 管理公开。|![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加声称说明](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![创建索赔规则](media/icon_checkboxo.gif)|根据你的组织的需求，创建一个或多个索赔规则规则集，以便将相应地发布索赔与此信赖的方信任关联。|![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建规则应用于穿透或传入声称筛选](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)<br /><br />![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建规则应用于作为索赔发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)<br /><br />![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建到作为索赔发送组成员规则](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)<br /><br />![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建规则转换传入声称](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)<br /><br />![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建发送身份验证方法声称规则](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)<br /><br />![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建发送广告 FS 规则 1.x 兼容声称](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)<br /><br />![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建自定义规则的发送索赔使用规则](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)|  
|![创建索赔规则](media/icon_checkboxo.gif)|根据你的组织的需求，创建一个或多个设置的颁发授权规则或委派授权规则设置，以便用户将到依赖方允许访问与此信赖的方信任关联的索赔规则。|![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建允许所有用户规则](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)<br /><br />![创建声称规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[传入声称上创建规则应用于拒绝用户基于或许可](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)|  
