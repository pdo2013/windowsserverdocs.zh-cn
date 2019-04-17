---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: "了解关键 Active Directory 联合身份验证服务概念"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>了解关键广告 FS 概念
建议你了解有关的重要的概念的 Active Directory 联合身份验证服务熟悉其功能集。  
  
> [!TIP]  
> 你可以找到其他广告 FS 资源的链接[广告 FS 内容地图](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx)Microsoft TechNet Wiki 上的页面。 本页由广告 FS 社区的成员，并由广告 FS 产品团队定期监控。  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>使用本指南中的广告 FS 术语  
  
|广告 FS 词|定义|  
|--------------|--------------|  
|帐户合作伙伴公司|由索赔在联合身份验证服务提供商信任联盟合作伙伴公司。 帐户合作伙伴公司包含将访问 Web\ 基于资源伙伴中的应用程序的用户。|  
|帐户联盟服务器|在帐户合作伙伴公司联盟服务器。 帐户联盟服务器用户基于用户身份验证问题安全标记。 服务器对用户身份验证、提取相关特性和退出特性官方商城的组成员信息、包索赔，插入此信息并生成和登录安全令牌 \（包含 claims\）若要返回到用户，以在自己的组织中使用，或者将发送到合作伙伴公司。|  
|广告 FS 配置数据库|用来存储配置的所有数据表示数据库单个广告 FS 实例或联合身份验证服务。 此配置数据可以存储 SQL Server 数据库中，或使用的 Windows 内部数据库功能附带 Windows Server 2016、Windows Server 2012 和 2012 R2 和 Windows Server 2008 和 2008 R2。 </br></br>你可以创建广告 FS 配置数据库 SQL Server 使用 Fsconfig.exe command\ 行工具和使用广告 FS 联盟服务器配置向导的 Windows 内部数据库。|  
|索赔提供程序|提供给它的用户的索赔组织。 请参阅帐户合作伙伴公司。|  
|索赔提供商信任|广告 FS snap\ 中管理中, 索赔提供商信任是信任通常资源合作伙伴公司代表信任关系访问在其帐户中的资源资源合作伙伴公司中的组织中创建的对象。 索赔提供商信任对象包含各种标识符、名称和规则来标识该合作伙伴本地联合身份验证服务。|  
|本地索赔提供商信任|代表广告 LDS 或广告 FS 场中的 third\ 方 LDAP\ 基于目录信任对象。 本地索赔对象包含各种标识符、名称和规则来标识该 LDAP\ 基于目录添加到本地联合身份验证服务提供商信任。|  
|联合元数据|数据配置信息索赔提供商和依赖方，便于实施的索赔提供商信任和信赖的方信任的正确配置之间进行通信的格式。 数据格式定义安全肯定标记语言 \(SAML\) 2.0、中和 WS\ 联盟扩展。|  
|联合身份验证的服务器|已使用广告 FS 联盟服务器配置向导联合身份验证的服务器角色中采取措施配置 Windows Server。 联合服务器问题标记，并作为联合身份验证服务的一部分。|  
|联合身份验证的服务器代理服务器|已使用广告 FS 联合身份验证的代理配置向导用作配置 Windows Server 中间的代理 Internet 客户端和位于防火墙公司的网络上的联合身份验证服务之间的服务。|  
|主要联合身份验证的服务器|Windows Server 已经配置了联合身份验证的服务器角色使用广告 FS 联盟服务器配置向导中，并且具有广告 FS 配置数据库 read\/写入副本。 </br></br> 当你使用广告 FS 联盟服务器配置向导并选择的选项，可以创建新的联合身份验证服务并场中使该计算机的第一个联合身份验证的服务器时创建主要联合身份验证的服务器。 此电场的日落中的所有其他联盟服务器必须复制到存储在本地广告 FS 配置数据库仅 read\ 副本主要联盟服务器上所做的更改。 广告 FS 配置数据库存储 SQL 数据库中，因为所有联合身份验证的服务器可以同样读取和写入 SQL Server 上存储的配置数据库时的条款"主要联合服务器"不适用于。|  
|依赖方|组织来接收并处理索赔。 查看资源合作伙伴组织。|  
|依赖方信任|在 snap\ 内的广告 FS 管理，依赖方信任信任对象通常创建中：<br /><br />合作伙伴公司代表组织信任关系访问在其帐户中的资源资源合作伙伴公司中的帐户。<br />的代表联合身份验证服务和单个 web\ 基于应用程序之间信任的资源合作伙伴公司。<br /><br />信赖的方信任对象包含各种标识符、名称和规则来标识该合作伙伴或 web\ 应用程序到本地联合身份验证服务。|  
|资源联盟服务器|在资源合作伙伴公司联盟服务器。 资源联盟服务器通常根据帐户联合身份验证的服务器发送一个安全标记用户问题安全标记。 服务器接收安全标记，验证签名、来繁衍所需的传出索赔取消打包索赔均适用索赔规则逻辑、生成一个新的安全标记 \（与传出 claims\) 以传入安全标记，以信息，并登录的新标记，以返回到用户和最终于的 Web 应用。|  
|资源合作伙伴公司|由联合身份验证服务信赖的方信任联盟合作伙伴。 资源合作伙伴颁发 claims\ 基于安全令牌包含已发布基于 Web\ 的应用程序可以访问在帐户合作伙伴的用户。|  
  
## <a name="overview-of-ad-fs"></a>广告 FS 的概述  
广告 FS 是提供的客户端计算机标识访问解决方案 \（内部或外部你 network\）无缝的 SSO 获取的受保护 Internet \-facing 应用程序或服务，即使用户帐户和应用程序位于完全不同的网络或组织。  
  
当某个应用或服务处于一个网络，并且用户帐户在其他网络时，通常用户是时提示输入凭据辅助他或她尝试访问的应用程序或服务。 这些辅助凭据表示应用程序或服务在所在的领域中的用户的身份。 它们通常所需的 Web 服务器的承载应用程序或服务，以便它可以进行最适合授权决策。  
  
与广告 FS 组织可以跳过辅助凭据请求提供信任关系 \(federation trusts\)，可使用这些组织投影受信任的合作伙伴对用户的数字身份和访问权限。 在此联盟环境中，每一个组织继续管理其自己的身份，但每一个组织牢固还可以投影并接受身份的其他机构。  
  
-   [特性官方商城的作用](The-Role-of-Attribute-Stores.md)  
  
-   [广告 FS 配置数据库的作用](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [索赔的作用](The-Role-of-Claims.md)  
  
-   [角色的索赔规则](The-Role-of-Claim-Rules.md)  
  
-   [角色的索赔引擎](The-Role-of-the-Claims-Engine.md)  
  
-   [索赔管道的作用](The-Role-of-the-Claims-Pipeline.md)  
  
-   [角色的索赔规则语言](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [确定要使用的索赔规则模板类型](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [在广告 FS 如何使用 Uri](How-URIs-Are-Used-in-AD-FS.md)  
  

