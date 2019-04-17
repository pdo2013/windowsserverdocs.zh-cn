---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: "何时使用发送组成员作为索赔规则"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10e027b4de463aad2b05eae40a622aebf8f12a3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>何时使用发送组成员作为索赔规则
你可以在 Active Directory 联合身份验证服务 \(AD FS\) 使用此规则有时候你要指定的活动目录安全组成员的用户的新传出索赔值的问题。 当你使用此规则时，你发出仅组单个索赔指定和相匹配的规则逻辑下, 表中所述。  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|传出声明值|如果用户组成员等于*指定的组*和传出声明类型等于*指定索赔类型*，然后替换与的现有组价值*指定传出声称值*并发出的索赔。|  
  
下面提供了基本简介声称规则。 它们还提供了有关何时用作索赔规则发送组成员资格的详细信息。  
  
## <a name="about-claim-rules"></a>有关索赔规则  
索赔规则表示企业逻辑，将需要传入的索赔，到该应用条件实例 \（如果然后 y\ x）和产生传出声明条件参数而异。 以下列表轮廓重要阅读进一步之前，你应该提前了解的提示声称规则本主题中：  
  
-   在 snap\ 中广告 FS 管理索赔规则只能创建使用索赔规则模板  
  
-   索赔规则进程传入索赔直接从索赔提供商 \（如 Active Directory 或其他联盟 Service\）或的输出中验收转换上索赔提供商信任规则。  
  
-   索赔规则处理给定的规则组内的时间顺序索赔颁发引擎。 通过在规则设置优先级，可以进一步优化或筛选将生成一给定的规则组中的以前规则的索赔。  
  
-   索赔规则模板将始终需要你指定传入的索赔类型。 但是，你可以使用相同的索赔类型使用单个规则处理多个索赔值。  
  
有关详细信息索赔规则和索赔规则集，请参阅[声称规则的角色](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。 如何处理声明规则集的详细信息，请参阅[的索赔管道角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="outgoing-claim-value"></a>传出声明值  
作为索赔规则模板使用发送组成员资格，可以发出是取决于用户是否是你所指定的一组成员的索赔。  
  
换言之，仅当用户可以组安全的索赔此规则模板发出 ID \(SID\) 相匹配管理员指定的 Active Directory 组。 身份验证的 Active Directory 域服务 \(AD DS\) 所有用户都将都有传入组 SID 宣称属于每组。 默认情况下，在 Active Directory 索赔提供商信任验收转换规则通过这些组 SID 索赔。 使用的基础发出索赔 Sid 速度较快比用户的多个组广告 DS 中查找这些组。  
  
当你使用此规则，单个索赔已发送，根据你选择的 Active Directory 组。 例如，可以使用此规则模板创建将其发送组索赔值为"管理员"用户是否的域管理员安全组成员规则。  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>配置上的索赔提供商信任此规则  
管理员应在仅当组 Sid 正在接收索赔提供商，这是非常少见除 Active Directory 或广告 DS 任何索赔提供商时使用此规则类型索赔提供商信任验收转换规则。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
创建使用本规则索赔规则语言或通过发送 LDAP 组成员资格用作索赔规则中 snap\ 中广告 FS 管理模板。 此规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   选择用户组使用对象选取器  
  
-   选择传出的索赔类型  
  
-   选择传出的名字 ID 格式 \（这可能可用仅名称 ID 选择时从传出索赔类型 field\）  
  
-   指定传出的索赔值  
  
有关如何创建此规则的详细信息，请参阅[创建规则应用于作为索赔发送组成员](https://technet.microsoft.com/en-us/library/ee913569.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用索赔规则语言  
如果你想要发出根据一组 SID 以外 SID 传入的索赔，使用转换传入的索赔规则模板。 如果管理员想要检索所有组成员的用户的名称，请改用发送 LDAP 属性为索赔规则模板与**tokenGroups**属性。  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>示例：如何处理基于用户的组成员组声明  
以下规则发出组提起的索赔根据 SID 传入组的用户：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>其他参考  
[创建规则作为索赔发送 LDAP 属性](https://technet.microsoft.com/library/dd807115.aspx)  
  

