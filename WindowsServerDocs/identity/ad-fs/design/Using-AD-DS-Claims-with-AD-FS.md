---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: AD FS 部署拓扑注意事项
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838378"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>将 AD DS 声明用于 AD FS
  
>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012
  
你可以通过使用 Active Directory 域服务启用更丰富的联合应用程序的访问控制\(AD DS\)\-颁发与 Active Directory 联合身份验证服务的用户和设备声明\(AD FS\).  
  
## <a name="about-dynamic-access-control"></a>有关动态访问控制  
在 Windows Server® 2012年动态访问控制功能使组织能够授予用户声明的基础的文件访问权限\(这由用户帐户属性中指明其出处\)和设备声明\(其来源的计算机帐户属性\)Active Directory 域服务颁发\(AD DS\)。 发出声明，AD DS 已集成到 Windows 集成身份验证通过 Kerberos 身份验证协议。  
  
有关动态访问控制的详细信息，请参阅[动态访问控制内容指南](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
### <a name="whats-new-in-ad-fs"></a>什么是 AD FS 中的新增功能？  
作为动态访问控制方案的扩展，现在可以在 Windows Server 2012 中的 AD FS:  
  
-   访问计算机帐户以及从 AD DS 中的用户帐户属性的属性。 在以前版本的 AD FS，联合身份验证服务无法访问计算机帐户属性在所有 AD DS 中。  
  
-   使用 AD DS 颁发驻留在 Kerberos 身份验证票证的用户或设备声明。 在以前版本的 AD FS 中，声明引擎是能够读取用户和组安全 Id \(Sid\)从 Kerberos，但却不能读取任意声明的 Kerberos 票证中包含的信息。  
  
-   将 AD DS 为信赖的应用程序可用于执行更丰富的访问控制的 SAML 令牌中颁发用户或设备声明转换。  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>使用 AD DS 的好处与 AD FS 声明  
发出声明，这些 AD DS 可以插入到 Kerberos 身份验证票证，并可以与 AD FS 配合使用来提供以下优势：  
  
-   需要更丰富的访问控制策略的组织可以启用声明\-基于的对应用程序和资源使用 AD DS 颁发基于给定的用户或计算机帐户的 AD DS 中存储的属性值的声明。 这可帮助管理员减少与创建和管理相关的额外开销：  
  
    -   否则将使用用于控制对应用程序和可通过 Windows 集成身份验证访问的资源访问的 AD DS 安全组。  
  
    -   林信任，否则将用于控制对业务的访问\-到\-业务\(B2B\) \/ Internet 可访问应用程序和资源。  
  
-   组织可以现在防止未经授权的访问网络资源从客户端计算机基于特定的计算机帐户是否属性值存储在 AD DS\(例如，计算机的 DNS 名称\)匹配的访问控制资源策略\(例如，已在声明已纳入 Acl 的文件服务器\)或信赖方策略\(例如，声明\-感知 Web 应用程序\)。 这可以帮助管理员设置的资源或应用程序是更精细的访问控制策略：  
  
    -   仅可通过 Windows 集成身份验证访问。  
  
    -   可通过 AD FS 身份验证机制访问 Internet。 可以使用 AD FS 来转换为可封装到可供 Internet 可访问的资源或信赖方应用程序的 SAML 令牌的 AD FS 声明发出设备声明，AD DS。  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>AD DS 和 AD FS 之间的差异发出声明，  
有两个区别因素非常重要，要了解从 AD DS vs 颁发声明的。AD FS。 这些差异包括：  
  
-   AD DS 只能颁发 Kerberos 票证，不 SAML 令牌中封装的声明。 有关 AD DS 如何发出声明的详细信息，请参阅[动态访问控制内容指南](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
-   AD FS 只能颁发 SAML 令牌，不是 Kerberos 票证中封装的声明。 有关如何将 AD FS 发出声明的详细信息，请参阅[The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)。  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>AD DS 颁发的声明与 AD FS 工作的方式  
AD DS 发出声明，可以与 AD FS 配合使用来访问直接从用户的身份验证上下文，而不是单独的 LDAP 调用到 Active Directory 用户和设备声明。 下图和相应的步骤介绍了此过程中启用声明的更多详细信息的工作方式\-基于动态访问控制方案的访问控制。  
  
![使用声明](media/UsingADDSClaimswithADFS.gif)  
  
1.  AD DS 管理员在 AD DS 架构中使用 Active Directory 管理中心控制台或 PowerShell cmdlet 来启用特定的声明类型的对象。  
  
2.  AD FS 管理员使用 AD FS 管理控制台来创建和配置的声明提供程序和信赖方信任关系 pass\-通过或转换声明规则。  
  
3.  Windows 客户端尝试访问网络。 为 Kerberos 身份验证过程的一部分，客户端提供其用户和计算机票证\-授予票证\(TGT\)其中尚不包含任何声明，到域控制器。 域控制器然后启用的声明类型，将在 AD DS 中查找并返回的 Kerberos 票证中包含任何生成的声明。  
  
4.  当用户\/客户端尝试访问的文件资源是已纳入 Acl，需要声明，它们可以访问该资源，因为从 Kerberos 显示复合 ID 具有这些声明。  
  
5.  当同一个客户端尝试访问配置为 AD FS 身份验证的网站或 Web 应用程序时，则会将用户重定向到配置为 Windows 集成身份验证的 AD FS 联合身份验证服务器。 客户端将请求发送到使用 Kerberos 的域控制器。 域控制器颁发 Kerberos 票证包含所请求的声明，该客户端以提供给联合身份验证服务器。  
  
6.  基于声明提供程序配置的声明规则的方式和信赖方信任，以前配置的管理员，AD FS 声明从读取的 Kerberos 票证并将它们包含在 SAML 令牌中颁发的客户端。  
  
7.  客户端接收包含正确的声明的 SAML 令牌和重定向到该网站。  
  
有关如何创建所需的 AD DS 颁发声明为使用 AD FS 声明规则的详细信息，请参阅[创建一个规则以转换传入声明](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
