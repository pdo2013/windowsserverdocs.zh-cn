---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: 何时使用发送组成员身份作为声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 82dd9cec2c75a796eb0def508082508a5d0dbf5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385434"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>何时使用发送组成员身份作为声明规则
如果希望为仅属于指定 Active Directory \(安全\)组成员的用户发出新的传出声明值，则可以在 Active Directory 联合身份验证服务 AD FS 中使用此规则。 使用此规则时，你只会为与规则逻辑匹配的指定组发出单个声明，如下表中所述。  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|传出声明值|如果用户的组成员身份等于*指定的组*，并且传出声明类型等于*指定的声明类型*，则将现有组名称值替换为*指定的传出声明值*并发出声明。|  
  
以下部分提供声明规则的基本简介。 它们还提供有关何时使用发送组成员身份作为声明规则的详细信息。  
  
## <a name="about-claim-rules"></a>关于声明规则  
声明规则表示将接受传入声明的业务逻辑实例，如果 x 之后为 y \(\) ，则对其应用条件，并基于条件参数生成传出声明。 下面的列表概述了在进一步阅读本主题中的内容之前应了解的有关声明规则的重要提示：  
  
-   在 AD FS 管理 "管理\-单元中，只能使用声明规则模板创建声明规则  
  
-   声明规则直接从声明提供程序\(（例如 Active Directory 或另一个联合身份验证服务\) ）或在声明提供方信任的接受转换规则的输出中处理传入声明。  
  
-   声明规则由声明颁发引擎按给定规则集内的时间顺序处理。 通过为规则设置优先级，可以进一步优化或筛选由给定规则集内以前的规则生成的声明。  
  
-   声明规则模板始终要求你指定传入声明类型。 但是，你可以使用单个规则处理声明类型相同的多个声明值。  
  
有关声明规则和声明规则集的更多详细信息，请参阅[声明规则的角色](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[声明引擎的角色](The-Role-of-the-Claims-Engine.md)。 有关如何处理声明规则集的详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="outgoing-claim-value"></a>传出声明值  
通过使用发送组成员身份作为声明规则模板，你可以发出取决于用户是否为指定组的成员的声明。  
  
换句话说，此规则模板仅当用户的组安全 ID \(SID\)与管理员指定的 Active Directory 组匹配时才会发出声明。 对 Active Directory 域服务\(AD DS\)进行身份验证的所有用户都将具有其所属的每个组的传入组 SID 声明。 默认情况下，Active Directory 声明提供方信任中的接受转换规则通过这些组 SID 声明进行传递。 使用这些组 Sid 作为发出声明的基础比在 AD DS 中查找用户组要快得多。  
  
使用此规则时，仅根据选择的 Active Directory 组发送单个声明。 例如，可以使用此规则模板创建一个规则，该规则在用户是 Domain Admins 安全组的成员时会发送值为“Admin”的组声明。  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>对声明提供程序信任配置此规则  
仅当是从声明提供方接收组 SID（这对于除 Active Directory 或 AD DS 之外的任何声明提供方而言都是非常少见的），管理员才应在声明提供方信任的接受转换规则中使用此规则类型。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
你可以使用声明规则语言或使用 "AD FS 管理" 管理单元\-中的 "将 LDAP 组成员身份作为声明规则" 模板来创建此规则。 此规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   使用对象选取器选择用户的组  
  
-   选择传出声明类型  
  
-   选择仅当从 "传出\(声明类型" 字段中选择名称 id 时才可用的传出名称 id 格式\)  
  
-   指定传出声明值  
  
有关如何创建此规则的详细信息，请参阅[创建规则以将组成员身份作为声明发送](https://technet.microsoft.com/library/ee913569.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用声明规则语言  
如果要基于组 SID 之外的传入 SID 发出声明，请使用“转换传入声明”规则模板。 如果管理员要检索用户所属的所有组的名称，请使用“以声明方式发送 LDAP 属性”规则模板，而不是使用 **tokenGroups** 属性。  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>例如：如何基于用户的组成员身份发出组声明  
以下规则基于传入组 SID 为用户发出组声明：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>其他参考  
[创建规则以将 LDAP 属性作为声明发送](https://technet.microsoft.com/library/dd807115.aspx)  
  

