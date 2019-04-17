---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: "索赔的作用"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98765deaba67ffdc0ee18b6d8ef573e531d739cc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-claims"></a>索赔的作用
在 claims\ 基于身份模型，索赔播放关键作用联合身份验证过程中，它们的关键组件根据所有 Web\ 基于身份验证和授权请求的结果。 此模型，组织牢固投影数字身份和权利权利或*索赔*、在安全和标准化方式企业限制。  
  
## <a name="what-are-claims"></a>什么是索赔？  
简单来说，在索赔只是*明细表*\（例如，命名的身份，group\），所做的用户，主要用于授权向 Internet 上的任意位置 claims\ 基于应用程序访问有关。 每个声明对应于*值*存储在索赔。  
  
### <a name="how-claims-are-sourced"></a>如何确定索赔有源  
联合身份验证服务 Active Directory 联合身份验证服务 \(AD FS\) 中定义的索赔互换联盟的合作伙伴。 但是，可以执行此操作之前它首先必须填充或源检索的值或计算的值索赔。 每个声明值表示的用户，组中或实体的值，并为源在两种方式之一：  
  
1.  从 Active Directory 用户帐户的属性检索占据索赔值检索时从属性应用商店，例如，当销售部门特性值。 有关详细信息，请参阅[特性角色商店](The-Role-of-Attribute-Stores.md)。  
  
2.  当传入的声明的转换为另一个值根据表达规则中的逻辑。 例如，当传入声明的域管理员值转换为管理员新值发送作为传出声明之前。 有关详细信息，请参阅[索赔规则角色](The-Role-of-Claim-Rules.md)。  
  
索赔可以包括 e\ 邮件地址、用户主要名称 \(UPN\)、组成员和其他帐户属性等值。  
  
### <a name="how-claims-flow"></a>如何索赔流  
第三方依赖于索赔他们主持 Web\ 基于应用程序中执行授权任务的值。 这些方被称为*信赖方*广告 FS 管理 snap\ 中。 联合身份验证服务用于协调之间许多不同方信任负责。 它旨在处理和流索赔组织最初引起的索赔，也称为从受信任的 exchange*索赔提供商*广告 FS 管理 snap\ 加入，信赖方中。 依赖方使用这些声明做出决定授权。  
  
使用此流程的索赔流称为*索赔管道*。 流中的索赔通过索赔管道有三个步骤：  
  
1.  验收转换规则上索赔提供商信任处理来自索赔提供商的索赔。 这些规则确定哪些索赔接受索赔提供商。  
  
2.  验收转换规则输出用作颁发授权规则的输入。 这些规则确定用户是否允许访问依赖方。  
  
3.  验收转换规则输出用作颁发转换规则的输入。 这些世纪将发送到信赖方索赔。  
  
有关详细信息，请参阅[的索赔管道的作用](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>索赔颁发的方式  
编写索赔规则时, 传入提起的索赔索赔规则的来源而异索赔提供商信任或信赖的方信任是否你撰写规则。 编写索赔提供商信任的索赔规则时, 传入索赔是从受信任的索赔提供商发送到联合身份验证服务索赔。 编写信赖的方信任规则时, 传入的索赔是输出索赔规则适用索赔提供商信任的索赔。 有关索赔传入和传出声明的详细信息，请参阅[的索赔管道角色](The-Role-of-the-Claims-Pipeline.md)和[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。  
  
## <a name="what-are-claim-types"></a>索赔类型是什么？  
索赔类型提供的索赔值上下文。 统一资源标识符 \(URI\) 通常表示它。 广告 FS 可以支持任何索赔类型，并且默认情况下配置使用下表中的索赔类型。  
  
|名称|描述|URI|  
|--------|---------------|-------|  
|E\ 邮件地址|用户的 e\ 邮件地址|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress|  
|给定的名称|给定的用户的名称|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenname|  
|名称|用户的唯一的名称|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/name|  
|UPN|用户主要命名 \(UPN\) 的用户|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/upn|  
|常见的名称|常见的用户的名称|http:///\/schemas.xmlsoap.org\/claims\/CommonName|  
|广告 FS 1.x E\ 邮件地址|当与广告 FS 1.1 或 ADFS 1.0 互操作的用户 e\ 邮件地址|http:///\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|组|用户所在的一组|http:///\/schemas.xmlsoap.org\/claims\/Group|  
|广告 FS 1.x UPN|当与广告 FS 1.1 或 ADFS 1.0 进行交互时，用户 UPN|http:///\/schemas.xmlsoap.org\/claims\/UPN|  
|角色|用户可以角色|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/role|  
|姓氏|用户姓|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|专用的用户的标识符|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|名称标识符|用户 SAML 名称标识符|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/nameidentifier|  
|身份验证方法|用来用户身份验证的方法|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/authenticationmethod|  
|拒绝仅分组 SID|仅 deny\ SID 用户组|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|拒绝仅主要 SID|用户的仅 deny\ 主要 SID|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|拒绝主要的组 SID|仅 deny\ 主要 SID 用户组|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|组 SID|用户 SID 组|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/groupsid|  
|主要组 SID|用户主要组 SID|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|主要 SID|用户的主要 SID|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Windows 的帐户名称|在以下形式出现：用户域帐户名称<domain>\\<user>|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>什么是索赔说明？  
索赔说明代表广告 FS 支持和程序可能会发布联盟元数据中的索赔类型的列表。 在上一个表格中提到的索赔类型配置为索赔广告 FS 管理 snap\ 中的说明。  
  
广告 FS 配置数据库中存储的索赔描述将发布到联盟元数据的收集。 这些声称说明由各种组件联合身份验证服务。  
  
每个索赔说明包括 URI 的索赔类型、名称、发布的状态和描述。 你可以通过使用管理索赔描述集锦**声称说明**广告 FS 管理 snap\ 中节点。 你可以修改索赔描述使用 snap\ 发布状态。 以下设置有：  
  
-   **作为此联合身份验证服务可接受索赔类型联盟元数据中发布此声明**\(Publish as Accepted\) — 指示将被来自其他索赔提供商的此联合身份验证服务的索赔类型。  
  
-   **为此联合身份验证服务可发送的索赔类型联盟元数据中发布此声明**\(Publish as Sent\) — 表示此联合身份验证服务提供的索赔类型。 以下是联合身份验证服务发布给其他人与愿意发送的索赔类型。 实际索赔类型索赔提供商发送通常是子集的此列表。  
  
有关如何进行设置的索赔类型发布状态的详细信息，请参阅[添加声称描述](https://technet.microsoft.com/library/dd807051.aspx)广告 FS 部署指南中。  
  
### <a name="when-generating-federation-metadata"></a>当生成联盟元数据  
联盟元数据包括标记为发布的所有声明说明。  
  
### <a name="when-claims-rules-are-processed"></a>当处理索赔规则  
保持配置索赔描述的信息，时，您可以配置有关索赔规则更轻松。 有关可在索赔提供商组织的索赔规则的详细信息，请参阅[声称规则的角色](The-Role-of-Claim-Rules.md)。  
  

