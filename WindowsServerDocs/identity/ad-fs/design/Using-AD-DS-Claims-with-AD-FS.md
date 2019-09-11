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
ms.openlocfilehash: ba7ed6baa30d15eea5ae08326847187ac85605d5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867584"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>将 AD DS 声明用于 AD FS
  
  
可以通过将\(Active Directory 域服务 AD DS\)\-颁发的用户和设备声明与 Active Directory 联合身份验证服务\(结合使用，为联合应用程序启用更丰富的访问控制 AD FS\).  
  
## <a name="about-dynamic-access-control"></a>关于动态访问控制  
在 Windows Server®2012中，动态访问控制功能使组织能够根据用户帐户属性\(\)提供文件的访问权限， \(这些声明源自于Active Directory 域服务\) \(AD DS颁发的计算机帐户属性。\) AD DS 颁发的声明通过 Kerberos 身份验证协议集成到 Windows 集成身份验证。  
  
有关动态访问控制的详细信息，请参阅[动态访问控制内容路线图](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
### <a name="whats-new-in-ad-fs"></a>AD FS 中有哪些新功能？  
作为动态访问控制方案的扩展，现在可以在 Windows Server 2012 中 AD FS：  
  
-   除了 AD DS 中的用户帐户属性以外，还可以访问计算机帐户属性。 在以前版本的 AD FS 中，联合身份验证服务根本无法从 AD DS 访问计算机帐户属性。  
  
-   使用驻留在 Kerberos 身份验证票证中 AD DS 颁发的用户或设备声明。 在以前版本的 AD FS 中，声明引擎能够读取 kerberos 中的用户和组安全\(id\) sid，但无法读取任何包含在 kerberos 票证中的声明信息。  
  
-   将 AD DS 颁发的用户或设备声明转换为 SAML 令牌，这些令牌依赖于应用程序可用于执行更丰富的访问控制。  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>将 AD DS 声明与 AD FS 结合使用的好处  
这些 AD DS 颁发的声明可以插入 Kerberos 身份验证票证并与 AD FS 一起使用，以提供以下优势：  
  
-   如果组织需要更丰富的访问控制策略，\-AD DS 则可以通过使用 AD DS 颁发的声明来实现对应用程序和资源的基于声明的访问。 这可以帮助管理员减少与创建和管理相关的额外开销：  
  
    -   AD DS 安全组，该安全组将用于控制对可通过 Windows 集成身份验证访问的应用程序和资源的访问。  
  
    -   否则，林信任将用于控制对企业\-到\-企业\(B2B\) \/ Internet 可访问应用程序和资源的访问。  
  
-   组织现在可以根据 AD DS \(中存储的特定计算机帐户属性值（例如，计算机的 DNS 名称\)是否与访问控制匹配，阻止来自客户端计算机的网络资源的未经授权的访问资源\(策略例如，已已纳入 acl 声明\)或信赖方策略\(的文件服务器，例如，声明\-感知 Web 应用程序\)。 这可以帮助管理员为以下资源或应用程序设置更精细的访问控制策略：  
  
    -   仅可通过 Windows 集成身份验证访问。  
  
    -   通过 AD FS 身份验证机制可访问 Internet。 AD FS 可用于将 AD DS 颁发的设备声明转换为可以封装到 SAML 令牌中的 AD FS 声明，可供可通过 Internet 访问的资源或信赖方应用程序使用。  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>AD DS 和 AD FS 颁发的声明之间的差异  
有两个不同的因素需要了解从 AD DS 和颁发的声明AD FS。 这些差异包括：  
  
-   AD DS 只能发出在 Kerberos 票证中封装的声明，而不是 SAML 令牌。 有关 AD DS 如何发出声明的详细信息，请参阅[动态访问控制内容路线图](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
-   AD FS 只能发出封装在 SAML 令牌中的声明，而不是 Kerberos 票证。 有关 AD FS 如何发出声明的详细信息，请参阅[声明引擎的角色](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)。  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>AD DS 颁发的声明与 AD FS 工作的方式  
AD DS 颁发的声明可以与 AD FS 一起使用，以便直接从用户的身份验证上下文访问用户和设备声明，而不是对 Active Directory 进行单独的 LDAP 调用。 下图和相应的步骤讨论了此过程的工作原理，以便为动态\-访问控制方案启用基于声明的访问控制。  
  
![使用声明](media/UsingADDSClaimswithADFS.gif)  
  
1.  AD DS 管理员使用 Active Directory 管理中心控制台或 PowerShell cmdlet 来启用 AD DS 架构中的特定声明类型对象。  
  
2.  AD FS 管理员使用 AD FS 管理控制台来创建和配置声明提供者和信赖方信任，同时传递\-或转换声明规则。  
  
3.  Windows 客户端尝试访问网络。 在 Kerberos 身份验证过程中，客户端向域控制器提供其用户和\-计算机票证\(授予\)票证未包含任何声明的请求。 然后，域控制器会在 AD DS 中查找已启用的声明类型，并在返回的 Kerberos 票证中包含任何生成的声明。  
  
4.  当用户\/客户端尝试访问已纳入 acl 需要声明的文件资源时，他们可以访问该资源，因为从 Kerberos 呈现的复合 ID 具有这些声明。  
  
5.  当相同的客户端尝试访问为 AD FS 身份验证配置的网站或 Web 应用程序时，会将用户重定向到配置为使用 Windows 集成身份验证的 AD FS 联合服务器。 客户端使用 Kerberos 将请求发送到域控制器。 域控制器颁发包含请求的声明的 Kerberos 票证，客户端可以向联合服务器提供该票证。  
  
6.  根据在管理员以前配置的声明提供程序和信赖方信任上配置声明规则的方式，AD FS 从 Kerberos 票证中读取声明，并将其包含在它为客户端颁发的 SAML 令牌中。  
  
7.  客户端接收包含正确声明的 SAML 令牌，然后重定向到该网站。  
  
有关如何创建 AD DS 颁发的声明来处理 AD FS 所需的声明规则的详细信息，请参阅[创建转换传入声明的规则](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
