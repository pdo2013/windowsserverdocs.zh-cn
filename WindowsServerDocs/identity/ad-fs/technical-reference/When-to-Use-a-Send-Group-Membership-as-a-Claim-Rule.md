---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: 何时使用发送组成员身份作为声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dffd886ffd0bedd429918f72408b2d13d9fa1bdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859168"
---
>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>何时使用发送组成员身份作为声明规则
可以在 Active Directory 联合身份验证服务中使用此规则\(AD FS\)当你想要发出新的传出声明值只能是指定的 Active Directory 安全组的成员的用户。 使用此规则时，你只会为与规则逻辑匹配的指定组发出单个声明，如下表中所述。  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|传出声明值|如果用户的组成员身份等于“指定组”，并且传出声明类型等于“指定声明类型”，则将现有组名称值替换为“指定传出声明值”并发出声明。|  
  
以下部分提供声明规则的基本简介。 它们还提供有关何时使用发送组成员身份作为声明规则的详细信息。  
  
## <a name="about-claim-rules"></a>关于声明规则  
声明规则表示业务逻辑，使用传入声明、 向它应用条件的实例\(if x，then y\)和生成传出声明基于条件参数。 下面的列表概述了在进一步阅读本主题中的内容之前应了解的有关声明规则的重要提示：  
  
-   在 AD FS 管理管理单元\-中，声明只可以使用声明规则模板创建规则  
  
-   声明规则过程传入声明既可以直接从声明提供程序\(Active Directory 或另一个联合身份验证服务等\)或输出中的接受转换规则声明提供方信任上的。  
  
-   声明规则由声明颁发引擎按给定规则集内的时间顺序处理。 通过为规则设置优先级，可以进一步优化或筛选由给定规则集内以前的规则生成的声明。  
  
-   声明规则模板始终要求你指定传入声明类型。 但是，你可以使用单个规则处理声明类型相同的多个声明值。  
  
有关详细信息有关声明规则和声明规则集的详细信息，请参阅[规则的角色声明](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。 有关如何处理声明规则集的详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="outgoing-claim-value"></a>传出声明值  
通过使用发送组成员身份作为声明规则模板，你可以发出取决于用户是否为指定组的成员的声明。  
  
换而言之，此规则模板发出声明，只有用户有相应的组安全 ID 时\(SID\)匹配管理员指定的 Active Directory 组。 所有用户都对 Active Directory 域服务进行身份验证\(AD DS\)会传入组 SID 声明为其所属的每个组。 默认情况下，Active Directory 声明提供方信任中的接受转换规则通过这些组 SID 声明进行传递。 使用这些组 SID 作为发出声明的基础比在 AD DS 中查找用户组要快得多。  
  
使用此规则时，仅根据选择的 Active Directory 组发送单个声明。 例如，可以使用此规则模板创建一个规则，该规则在用户是 Domain Admins 安全组的成员时会发送值为“Admin”的组声明。  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>对声明提供程序信任配置此规则  
仅当是从声明提供方接收组 SID（这对于除 Active Directory 或 AD DS 之外的任何声明提供方而言都是非常少见的），管理员才应在声明提供方信任的接受转换规则中使用此规则类型。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
创建此规则使用声明规则语言或通过使用发送 LDAP 组成员身份作为声明规则模板位于 AD FS 管理管理单元\-中。 此规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   使用对象选取器选择用户的组  
  
-   选择传出声明类型  
  
-   选择传出名称 ID 格式\(位于仅当选择名称 ID 从传出声明类型字段\)  
  
-   指定传出声明值  
  
有关如何创建此规则的详细信息，请参阅[创建以声明方式发送组成员身份规则](https://technet.microsoft.com/library/ee913569.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用声明规则语言  
如果要基于组 SID 之外的传入 SID 发出声明，请使用“转换传入声明”规则模板。 如果管理员要检索用户所属的所有组的名称，请使用“以声明方式发送 LDAP 属性”规则模板，而不是使用 **tokenGroups** 属性。  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>例如：如何基于用户的组成员身份发出组声明  
以下规则基于传入组 SID 为用户发出组声明：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>其他参考  
[创建规则以声明方式发送 LDAP 属性](https://technet.microsoft.com/library/dd807115.aspx)  
  

