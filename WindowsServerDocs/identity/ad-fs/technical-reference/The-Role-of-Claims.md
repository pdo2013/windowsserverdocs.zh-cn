---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: 声明的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 851a70bbed606530ca8292f65bc4f776eae77fae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407344"
---
# <a name="the-role-of-claims"></a>声明的角色
在声明 @ no__t-0based 标识模型中，声明在联合身份验证过程中扮演 pivotal 角色，它们是确定所有 Web @ no__t-1based 身份验证和授权请求结果的关键组件。 此模型支持组织以一种标准化方法跨安全和企业界限安全地投影数字标识和权限或 *声明*。  
  
## <a name="what-are-claims"></a>什么是声明？  
作为最简单的形式，声明只是*语句*\(for 示例，name，identity，group @ no__t-2，这是有关用户的，主要用于授权访问位于 Internet 任意位置的声明 @ no__t 3based 应用程序。 每个语句对应于存储在声明中的*值*。  
  
### <a name="how-claims-are-sourced"></a>如何获取声明来源  
Active Directory 联合身份验证服务 \(AD FS @ no__t-1 中的联合身份验证服务定义在联合伙伴之间交换哪些声明。 但是，在它可以执行此操作前必须首先借助检索到的或计算的值填充或获取声明来源。 每个声明值表示用户、组或实体的值，并且以下列两种方式之一获取来源：  
  
1.  从属性存储中检索构成声明的值时，例如，从 Active Directory 用户帐户的特性中检索销售部门的属性值时。 有关详细信息，请参阅 [The Role of Attribute Stores](The-Role-of-Attribute-Stores.md)。  
  
2.  在传入声明的值转换为另一个基于在规则中表示逻辑的值时。 例如，当具有域管理员值的传入声明作为传出声明发送之前，转换为管理员的新值时。 有关详细信息，请参阅[声明规则的角色](The-Role-of-Claim-Rules.md)。  
  
声明可以包含如下所示的值： e @ no__t-0mail 地址、用户主体名称 \(UPN @ no__t、组成员身份和其他帐户属性。  
  
### <a name="how-claims-flow"></a>如何声明流  
其他各方依赖于声明的值为他们托管的 Web @ no__t-0based 应用程序执行授权任务。 这些参与方称为 AD FS 管理 snap @ no__t-1in 中的*依赖方*。 联合身份验证服务负责协调许多不同参与方之间的信任关系。 它旨在处理和流动来自最初源声明（也称为 AD FS 管理 snap @ no__t-1in 中的*声明提供程序*）的受信任声明交换，并将其传递给信赖方。 信赖方然后使用这些声明做出授权决定。  
  
使用此过程的声明流称为*声明管道*。 通过声明管道的声明流有三个步骤：  
  
1.  从声明提供方收到的声明按照声明提供方信任上的接受转换规则进行处理。 这些规则可确定哪些声明接受自声明提供方。  
  
2.  接受转换规则的输出用作颁发授权规则的输入。 这些规则可确定是否允许用户访问信赖方。  
  
3.  接受转换规则的输出用作颁发转换规则的输入。 这些规则可确定将发送到信赖方的声明。  
  
有关详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>声明颁发的方式  
在编写声明规则时，声明规则的传入声明源根据是在声明提供方信任还是信赖方信任上编写规则而不同。 在为声明提供方信任编写声明规则时，传入声明是从受信任声明提供方发送到联合身份验证服务的声明。 在为信赖方信任编写规则时，传入声明是由适用的声明提供方信任的声明规则输出的声明。 有关传入声明和传出声明的详细信息，请参阅 [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md) 和 [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  
## <a name="what-are-claim-types"></a>什么是声明类型？  
声明类型为声明值提供上下文。 它通常表示为统一资源标识符 \(URI @ no__t-1。 AD FS 可以支持任何声明类型，并在默认情况下使用下表中的声明类型进行配置。  
  
|名称|描述|URI|  
|--------|---------------|-------|  
|E @ no__t-0Mail Address|用户的 e @ no__t-0mail 地址|http： \/\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7emailaddress|  
|给定名称|用户的给定名称|http： \/\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7givenname|  
|名称|用户的唯一的名称|http： \/\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7name|  
|UPN|用户主体名称 \(UPN @ no__t|http： \/\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7upn|  
|公用名|用户的公用名|http： \/\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3CommonName|  
|AD FS 1.x E @ no__t-0Mail Address|与 AD FS 1.1 或 ADFS 1.0 进行互操作时用户的 e @ no__t-0mail 地址|http： \/\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3EmailAddress|  
|Group|用户是其成员的组|http： \/\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3Group|  
|AD FS 1.x UPN|与 AD FS 1.1 或 ADFS 1.0 互操作时用户的 UPN|http： \/\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3UPN|  
|Role|用户具有的角色|http： \/\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7role|  
|姓氏|用户的姓氏|http： \/\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7surname|  
|PPID|用户的专用标识符|http： \/\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7privatepersonalidentifier|  
|名称标识符|用户的 SAML 名称标识符|http： \/\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7nameidentifier|  
|身份验证方法|用于对用户进行身份验证的方法|http： \/\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7authenticationmethod|  
|“仅拒绝”组 SID|用户的 deny @ no__t-0only 组 SID|http： \/\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlysid|  
|“仅拒绝”主 SID|用户的 deny @ no__t-0only 主 SID|http： \/\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlyprimarysid|  
|“仅拒绝”主组 SID|用户的 deny @ no__t-0only 主组 SID|http： \/\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlyprimarygroupsid|  
|组 SID|用户的组 SID|http： \/\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7groupsid|  
|主组 SID|用户的主组 SID|http： \/\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7primarygroupsid|  
|主 SID|用户的主 SID|http： \/\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7primarysid|  
|Windows 帐户名|用户的域帐户名，格式为 \<domain @ no__t-1 @ no__t-2 @ no__t-3user @ no__t|http： \/\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>什么是声明说明？  
声明说明表示 AD FS 支持且可在联合元数据中发布的声明类型的列表。 上表中提到的声明类型在 AD FS 管理 snap @ no__t-0in 中配置为声明说明。  
  
将发布到联合元数据的声明说明集合存储在 AD FS 配置数据库中。 这些声明说明用于联合身份验证服务的各个组件。  
  
每个声明说明包括声明类型 URI、名称、发布状态和描述。 你可以使用 "AD FS 管理" 管理 no__t-1in 中的 "**声明说明**" 节点管理声明说明集合。 您可以使用 snap @ no__t-0in 修改声明说明的发布状态。 提供了下列设置：  
  
-   **在联合元数据中发布此声明作为声明类型，此联合身份验证服务可以 @no__t 接受该声明类型**作为已接受的 no__t-2，指示此联合身份验证服务将接受来自其他声明提供程序的声明类型。  
  
-   **在联合元数据中发布此声明作为声明类型，此联合身份验证服务可以将其发送**@no__t 为发送的 @ no__t-2，指示此联合身份验证服务提供的声明类型。 它们作为联合身份验证服务愿意发送的声明类型而发布给其他服务。 声明提供方发送的实际声明类型通常是此列表的子集。  
  
有关如何设置声明类型的发布状态的详细信息，请参阅 AD FS 部署指南中的[添加声明说明](https://technet.microsoft.com/library/dd807051.aspx)。  
  
### <a name="when-generating-federation-metadata"></a>在生成联合元数据时  
联合元数据包括标记为发布的所有声明说明。  
  
### <a name="when-claims-rules-are-processed"></a>在处理声明规则时  
在你保存有关声明说明的配置信息时，你将更轻松地配置有关声明规则。 有关可以在声明提供方组织中使用的声明规则的详细信息，请参阅[声明规则的角色](The-Role-of-Claim-Rules.md)。  
  

