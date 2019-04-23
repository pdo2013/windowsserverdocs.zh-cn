---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: 了解关键的 Active Directory 联合身份验证服务的概念
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878128"
---
>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>了解关键的 AD FS 概念
建议你为 Active Directory 联合身份验证服务了解的重要概念并熟悉其功能集。  
  
> [!TIP]  
> 可以在 Microsoft TechNet Wiki 上的 [AD FS 内容导航图](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) 页面中找到更多 AD FS 资源链接。 此页面由 AD FS 社区的成员管理，并由 AD FS 产品团队定期进行监视。  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>本指南中使用的 AD FS 术语  
  
|AD FS 术语|定义|  
|--------------|--------------|  
|帐户伙伴组织|由联合身份验证服务中的声明提供方信任表示的联合身份验证伙伴组织。 帐户伙伴组织包含将访问 Web 的用户\-基于资源伙伴中的应用程序。|  
|帐户联合服务器|帐户伙伴组织中的联合服务器。 帐户联合服务器基于用户身份验证向用户颁发安全令牌。 服务器对用户进行身份验证、 提取相关属性和组成员身份信息从属性存储，此信息打包的声明，并生成和对安全令牌进行签名\(其中包含的声明\)若要返回给用户，或者要在其自己的组织中使用，或者发送到伙伴组织。|  
|AD FS 配置数据库|用于存储所有表示单个 AD FS 实例或联合身份验证服务的配置数据的数据库。 此配置数据可以存储在 SQL Server 数据库或使用 Windows 内部数据库功能包括与 Windows Server 2016、 Windows Server 2012 和 2012 R2 和 Windows Server 2008 和 2008 R2。 </br></br>可以使用 Fsconfig.exe 命令的 SQL server 创建 AD FS 配置数据库\-行工具和 Windows 内部数据库使用 AD FS 联合身份验证服务器配置向导。|  
|声明提供方|向其用户提供声明的组织。 请参阅帐户伙伴组织。|  
|声明提供方信任|在 AD FS 管理管理单元\-要在中，声明提供方信任通常创建的信任对象在资源伙伴组织的信任关系中表示组织中谁的帐户访问的资源中的资源合作伙伴组织。 声明提供方信任对象包含各种标识符、名称和用于在本地联合身份验证服务中标识此伙伴的规则。|  
|本地声明提供程序信任|一个表示 AD LDS 或第三个的信任对象\-方 LDAP\-基于 AD FS 场中的目录。 本地声明提供方信任对象包含各种标识符、 名称和用于标识此 LDAP 规则的\-基于本地联合身份验证服务的目录。|  
|联合元数据|用于在声明提供方和信赖方之间通信配置信息，以便正确配置声明提供方信任和信赖方信任的数据格式。 安全断言标记语言中定义的数据格式\(SAML\) 2.0 中，并且它将扩展在 WS\-联合身份验证。|  
|联合服务器|使用 AD FS 联合身份验证服务器配置向导以充当联合身份验证服务器角色进行了配置 Windows Server。 联合服务器颁发令牌并充当联合身份验证服务的一部分。|  
|联合服务器代理|使用 AD FS 联合服务器代理配置向导来充当进行了配置 Windows Server 的 Internet 客户端和位于企业网络中防火墙后面的联合身份验证服务之间的中间代理服务。|  
|主联合服务器|Windows Server 的已配置联合身份验证服务器角色使用 AD FS 联合身份验证服务器配置向导中，并且具有读取\/编写的 AD FS 配置数据库副本。 </br></br> 在使用 AD FS 联合身份验证服务器配置向导并选择选项以创建新的联合身份验证服务并使该计算机成为服务器场中的第一个联合身份验证服务器时，被创建主联合服务器。 此服务器场中的所有其他联合身份验证服务器必须将复制到读取主联合服务器上所做的更改\-副本存储在本地的 AD FS 配置数据库。 由于所有联合服务器可以平等地读取和写入到存储在 SQL Server 上的配置数据库，AD FS 配置数据库存储在 SQL 数据库时，术语“主联合服务器”不适用。|  
|信赖方|接收和处理声明的组织。 请参阅资源伙伴组织。|  
|信赖方信任|在 AD FS 管理管理单元\-在中，信赖方信任是通常在中创建的信任对象：<br /><br />帐户伙伴组织表示组织中为其帐户访问资源伙伴组织中的资源的信任关系。<br />资源伙伴组织表示联合身份验证服务和一个单独的网站之间的信任\-基于应用程序。<br /><br />信赖方信任对象包含各种标识符、 名称和标识此伙伴或 web 的规则的\-本地联合身份验证服务的应用程序。|  
|资源联合服务器|资源伙伴组织中的联合服务器。 资源联合服务器通常基于由帐户联合服务器颁发的安全令牌来向用户颁发安全令牌。 服务器接收安全令牌、 验证签名，适用于未打包的声明，以生成所需的传出声明的声明规则逻辑、 生成新的安全令牌\(具有传出声明\)基于信息在传入的安全令牌中，并对要返回给用户的新令牌进行签名并最终移到 Web 应用程序。|  
|资源伙伴组织|由联合身份验证服务中的信赖方信任表示的联合身份验证伙伴。 资源伙伴颁发的声明\-基于安全令牌，其中包含已发布的 Web\-基于的帐户伙伴中的用户可以访问的应用程序。|  
  
## <a name="overview-of-ad-fs"></a>AD FS 概述  
AD FS 是客户端计算机提供一个标识访问解决方案\(用于在网络内部或外部\)无缝 SSO 访问权限受保护的 Internet\-面向应用程序或服务，即使用户帐户和应用程序位于完全不同的网络或组织。  
  
当应用程序或服务在一个网络中，而用户帐户处于另一个网络中时，用户尝试访问应用程序或服务时通常会被提示输入辅助凭据。 这些辅助凭据表示应用程序或服务所驻留的领域中的用户身份。 托管应用程序或服务的 Web 服务器通常需要使用它们，以便该服务器可以做出最适当的授权决策。  
  
使用 AD FS 时，组织可以通过绕过输入辅助凭据的请求提供信任关系\(联合身份验证信任\)这些组织可以使用用户的数字标识和访问权限投影以受信任合作伙伴。 在此联合环境中，每个组织继续管理自己的标识，但每个组织还可以安全地投影和接受来自其他组织的标识。  
  
-   [属性存储的角色](The-Role-of-Attribute-Stores.md)  
  
-   [AD FS 配置数据库的角色](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [声明的角色](The-Role-of-Claims.md)  
  
-   [声明规则的角色](The-Role-of-Claim-Rules.md)  
  
-   [声明引擎的角色](The-Role-of-the-Claims-Engine.md)  
  
-   [声明管道的角色](The-Role-of-the-Claims-Pipeline.md)  
  
-   [声明规则语言的角色](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [确定要使用的声明规则模板的类型](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [如何在 AD FS 中使用 Uri](How-URIs-Are-Used-in-AD-FS.md)  
  

