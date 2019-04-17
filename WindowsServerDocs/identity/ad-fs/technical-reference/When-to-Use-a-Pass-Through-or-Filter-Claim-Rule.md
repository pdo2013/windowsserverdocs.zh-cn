---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: "何时使用一种穿过或筛选索赔规则"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa205c46bf67dc25a55232b799bdd39fee4ac3c6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>何时使用一种穿过或筛选索赔规则
在你需要采取具体传入的索赔类型，然后应用将确定哪些输出应该会根据传入声明中的值操作时，你可以使用 Active Directory 联合身份验证服务 \(AD FS\) 中此规则。 当你使用此规则时，你将通过或筛选匹配如下表中所，根据你在规则配置的选项之一规则逻辑任何索赔。  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|通过所有索赔值|如果传入索赔类型等于*指定索赔类型*和重视等*任何值*，然后将传递到索赔|  
|只有特定声称值通过|如果接收的索赔类型等于*指定索赔类型*和值等*指定声明值*，然后将传递到索赔|  
|通过只有符合特定 e\ 邮件职务值的索赔值|如果接收的索赔类型等于*指定索赔类型*和值等*指定职务值*，然后将传递到索赔|  
|通过将启动带有特定值的索赔值|如果接收的索赔类型等于*指定索赔类型*值开头*指定声明值*，然后将传递到索赔|  
  
以下部分提供了基本的介绍声称规则，并提供有关何时可以使用此规则的更多详细信息。  
  
## <a name="about-claim-rules"></a>有关索赔规则  
索赔规则表示企业逻辑，将需要传入的索赔，到该应用条件实例 \（如果然后 y\ x）和产生传出声明条件参数而异。 以下列表轮廓重要阅读进一步之前，你应该提前了解的提示声称规则本主题中：  
  
-   在 snap\ 中广告 FS 管理索赔规则只能创建使用索赔规则模板  
  
-   索赔规则进程传入索赔直接从索赔提供商 \（如 Active Directory 或其他联盟 Service\）或的输出中验收转换上索赔提供商信任规则。  
  
-   索赔规则处理给定的规则组内的时间顺序索赔颁发引擎。 通过在规则设置优先级，可以进一步优化或筛选将生成一给定的规则组中的以前规则的索赔。  
  
-   索赔规则模板将始终需要你指定传入的索赔类型。 但是，你可以使用相同的索赔类型使用单个规则处理多个索赔值。  
  
有关详细信息索赔规则和索赔规则集，请参阅[声称规则的角色](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。 如何处理声明规则集的详细信息，请参阅[的索赔管道角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="pass-through-all-claim-values"></a>通过所有索赔值  
使用此操作时，指定的索赔类型的所有声明值通过都传递作为传出索赔。 例如，当传入的索赔类型指定为角色索赔类型，所有传入的索赔值被单独复制到新的角色传出索赔类型的传出索赔。  
  
## <a name="filtering-a-claim"></a>筛选索赔  
广告 FS，术语*索赔筛选*意味着来筛选或限制传入声称值，以便传入或传出索赔作为通过发送仅某些值。 它是**穿透或筛选器传入的声明**规则模板，使此功能。 在此规则的属性，你可以设置条件，以便通过传递符合你指定的标准值筛选传入值。  
  
例如，你可以使用此规则仅通过时传入声称索赔类型的角色，或者你可能想要发出仅索赔有关用户的名称，类型匹配的购买者索赔值匹配的索赔，但不是包含身份证号的用户的索赔。  
  
当你使用此规则使用筛选条件时，传入的所有声明将进行都检查，以确定哪些索赔符合条件的规则设置。 所有其他索赔，以便可通过仅指定的索赔与所选的索赔类型匹配的值忽略。  
  
例如，在下图所示，当规则设置的条件筛选仅传入 UPN 有根据的索赔声称类型并还结尾@fabrikam.com，除非它们满足此条件忽略所有其他传入的索赔。 这包括传入声明索赔 E\ 邮件地址的类型，即使在结束其声明值@fabrikam.com。 在此情况下，仅包含值的索赔Nick@fabrikam.com发送到信赖方。  
  
![何时使用 pass 通过](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>配置上的索赔提供商信任此规则  
当你使用的索赔提供商信任时，可配置此规则通过仅传入匹配某些约束索赔提供商的索赔。 例如，你可能希望仅接受 e\ 邮件索赔索赔商;因此，你会使用此规则模板接受结束索赔提供商域名系统 \(DNS\) 名称中的 e\ 邮件索赔类型。  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>在信赖的方信任配置此规则  
当你使用信赖的方信任时，可配置此规则通过或筛选传出将发送到信赖方的索赔。 某些信赖方可能不会理解某些索赔类型，或某些索赔可能包含应该不会发送给某些信赖方的敏感信息。 此规则模板有助于以执行这些特定信赖的方信任的策略。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
创建该使用规则的索赔规则语言或穿透使用或筛选 snap\ 中广告 FS 管理传入声称规则模板。 此规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   指定传入的索赔类型  
  
-   通过所有索赔值  
  
-   只有特定声称值通过  
  
-   通过只有符合特定 e\ 邮件职务值的索赔值  
  
-   通过将启动带有特定值的索赔值  
  
有关如何创建此模板相关说明进行操作，请参阅[创建规则应用于穿透或筛选传入声明](https://technet.microsoft.com/library/dd807060.aspx)广告 FS 部署指南中。  
  
## <a name="using-the-claim-rule-language"></a>使用索赔规则语言  
如果仅在声明值匹配自定义模式时，应该发送索赔，必须使用自定义规则。 有关详细信息，请参阅何时使用自定义规则。  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>有关如何构建一种穿过或筛选的示例规则语法  
简单筛选规则会筛选索赔根据属性上述之一。 例如，通过所有 e\ 邮件索赔还将通过以下规则：  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
筛选器可逻辑和之间 ed 在一起。 例如，以下规则将接受所有 e\ 邮件索赔值 johndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
在上方的示例了筛选器始终使用相等运营商。 索赔规则语言支持以下运营商：  
  
-   \ = \ = \-等于 \(case\-sensitive\)  
  
-   \！\ = \-不等于 \(case\-sensitive\)  
  
-   \ = 约 \-常规 expression 匹配  
  
-   \！~ \-常规 expression non\ 匹配项  
  
例如，以下规则将接受所有 e\ 邮件索赔本地联盟服务器不发出具有的 boeing.com 职务：  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>创建自定义规则的最佳实践  
下表中所述，则可以将筛选器应用于一项或多个每个索赔，属性。  
  
|声称属性|描述|  
|------------------|---------------|  
|键入|索赔类型 \（通常为 Uri\ 表示）反映之间有关哪些类型的信息传达中索赔联合身份验证的合作伙伴隐式协议。 例如，键入 http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress 的索赔包含的用户的 e\ 邮件地址。|  
|值|此声明的值。 例如，键入 http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress 的索赔可能有值 johndoe@fabrikam.com|  
|值|值表示解释声明的值中包含的信息的方式。 通常都将值设置为 http:///\/www.w3.org\/2001\/XMLSchema\#string，但声明值可能包含 Base64Binary 编码数据 \ (例如，image\) 或日期、布尔和等等。|  
|发行商|发行商代表上次发出有关用户索赔方。 如果在索赔提供商联盟服务器上的所有声明发行商要设置为"本地机构"获得的索赔。 如果收到了联盟提供商联合服务器索赔的索赔发行商即将设置为登录该标记的索赔提供的索赔提供商标识符。 因此时处理收到索赔提供商的索赔规则, 发行商的所有声明将设置为相同的值。 对于信赖方创作规则时, 发行商属性可以用于区分来自不同的索赔提供商的索赔。|  
|OriginalIssuer|此声明属性旨在传达哪个联合身份验证的服务器最初颁发的索赔。 由于发行商的索赔设置的最后一登录该标记的联合服务器，原始发行商是非常有用的索赔已通过多个联合身份验证的服务器的排列了 \ (例如，从联盟提供商联盟服务器接收令牌依赖方可能感兴趣的特定索赔提供联合身份验证的服务器身份验证 user\)|  
|属性|除了上面列出的五个属性，每个索赔还有命名的属性存储属性包。 这些属性不在令牌序列，并且仅意义传递之间的单个联盟服务器的范围内索赔颁发管道组件的信息。 例如，设置期间索赔提供商规则处理，然后指的它在信赖方规则。|  
  

