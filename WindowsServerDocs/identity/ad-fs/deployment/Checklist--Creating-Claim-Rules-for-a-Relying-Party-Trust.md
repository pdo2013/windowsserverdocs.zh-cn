---
ms.assetid: 44271f44-b50a-4bce-9375-4fcab9618048
title: 清单-创建信赖方的声明规则信任
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9c75cd4ccbafefdda83cba4551fd6b9af63c4822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817398"
---
# <a name="checklist-creating-claim-rules-for-a-relying-party-trust"></a>清单：为信赖方信任创建声明规则

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

此清单包括所需的规划、 设计时，任务和部署声明与 Active Directory 联合身份验证服务中的信赖方信任相关联的规则\(AD FS\)。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![创建声明规则](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**核对清单：创建声明规则集为信赖方信任**  
  
||任务|参考|  
|-|--------|-------------|  
|![创建声明规则](media/icon_checkboxo.gif)|查看有关声明的概念、 声明规则、 声明规则集，并声明规则模板以及它们如何与联合信任相关联。|![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[角色声明](../../ad-fs/technical-reference/The-Role-of-Claims.md)<br /><br />![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规则的角色声明](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)|  
|![创建声明规则](media/icon_checkboxo.gif)|查看有关声明的流动方式声明颁发管道中的所有阶段和声明颁发引擎如何处理规则的概念。|![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[声明管道的角色](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)<br /><br />![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[声明引擎的角色](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)|  
|![创建声明规则](media/icon_checkboxo.gif)|若要有效地规划和实现将通过此信赖方信任颁发输出声明，确定是否需要一个或多个声明规则，该声明的规则，您应使用与此信赖方信任。|![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定类型的声明规则使用的模板](../../ad-fs/technical-reference/Determine-the-Type-of-Claim-Rule-Template-to-Use.md)|  
|![创建声明规则](media/icon_checkboxo.gif)|若要创建一个声明于另一个规则和如何使用声明规则语言提供比标准规则更复杂的逻辑才能提供理想的输出中所需的结果声明集时，请查看概念有关。|![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用经历或筛选器声明规则](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)<br /><br />![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用转换声明规则](../../ad-fs/technical-reference/When-to-Use-a-Transform-Claim-Rule.md)<br /><br />![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时发送 LDAP 属性用作声明规则](../../ad-fs/technical-reference/When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)<br /><br />![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用发送组成员身份作为声明规则](../../ad-fs/technical-reference/When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)<br /><br />![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)<br /><br />![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时使用自定义声明规则](../../ad-fs/technical-reference/When-to-Use-a-Custom-Claim-Rule.md)<br /><br />![创建声明规则](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[声明规则语言的角色](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)|  
|![创建声明规则](media/icon_checkboxo.gif)|如果尚不存在，则必须创建索赔说明，将满足您的组织的需求。 AD FS 附带了一组默认的公开的声明说明在 AD FS 管理管理单元中\-中。|![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加声明说明](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![创建声明规则](media/icon_checkboxo.gif)|根据您的组织的需要，创建规则集，以便将相应地颁发的声明与此信赖方信任相关联的一个或多个声明规则。|![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建传递规则或筛选传入声明](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)<br /><br />![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建规则以声明方式发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)<br /><br />![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建以声明方式发送组成员身份规则](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)<br /><br />![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建规则，以转换传入声明](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)<br /><br />![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建一个规则以发送身份验证方法声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)<br /><br />![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建一个规则以发送 AD FS 1.x 兼容声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)<br /><br />![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建规则发送声明使用自定义规则](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)|  
|![创建声明规则](media/icon_checkboxo.gif)|根据您的组织的需要，创建颁发授权规则集或委派授权规则集，以便用户将给依赖方允许访问此信赖方信任相关联的一个或多个声明规则。|![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建规则，以便允许所有用户](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)<br /><br />![创建声明规则](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[传入声明上创建允许或拒绝用户基于规则](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)|  
