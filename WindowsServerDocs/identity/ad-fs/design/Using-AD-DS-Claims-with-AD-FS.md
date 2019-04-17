---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: "广告 FS 部署拓扑注意事项"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>使用广告 DS 索赔与广告 FS
  
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012
  
你可以通过使用 Active Directory 域服务 \(AD DS\)\-issued 用户和设备索赔与 Active Directory 联合身份验证服务 \(AD FS\) 启用更丰富的联合应用访问控制。  
  
## <a name="about-dynamic-access-control"></a>有关动态访问控制  
在 Windows Server® 2012、 动态访问控制功能可以让组织授予访问文件根据用户索赔 \ （这来源的用户帐户 attributes\） 和设备索赔 \ （其中来源计算机帐户 attributes\ 通过） 颁发的 Active Directory 域服务 \(AD DS\)。 广告 DS 颁发索赔集成到 Windows 集成身份验证 （通过 Kerberos 身份验证协议。  
  
有关动态访问控制的详细信息，请参阅[动态访问控制内容路线图](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
### <a name="whats-new-in-ad-fs"></a>广告 FS 中的新增功能是什么？  
作为动态访问控制方案扩展，现在可以在 Windows Server 2012 的广告 FS:  
  
-   除了从内的广告 DS 的用户帐户属性访问计算机帐户属性。 在以前版本的广告 FS，联合身份验证服务无法访问计算机帐户属性根本从广告 DS。  
  
-   消耗广告 DS 颁发用户或设备位于 Kerberos 身份验证票证的索赔。 在以前版本的广告 FS，索赔引擎无法读取用户和组安全 Id \(SIDs\) 从 Kerberos 但无法阅读任何索赔 Kerberos 票证中包含的信息。  
  
-   转换广告 DS 为信赖的应用程序可用于执行更丰富访问控制的 SAML 标记颁发用户或设备的索赔。  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>使用广告 DS 好处索赔与广告 FS  
颁发索赔这些广告 DS 可以插入到 Kerberos 身份验证票证，并与广告 FS 用于提供以下优势：  
  
-   需要更丰富访问控制策略的组织可以通过使用广告 DS 颁发根据广告 DS 特定的用户或计算机帐户中存储的特性值的索赔启用 claims\ 基于访问应用和资源。 这可以帮助减少与创建和管理相关的其他开销管理员：  
  
    -   否则想用于控制访问权限的应用程序和都可以访问在通过集成 Windows 身份验证的资源的广告 DS 安全组。  
  
    -   林，否则将用于控制 Business\ to\ 业务 \(B2B\) 访问信任 \ / Internet 访问的应用程序和资源。  
  
-   组织可以现在防止未经授权的访问网络资源从客户端计算机上特定计算机帐户是否特性广告 DS 中存储的值 \ (例如的计算机 DNS name\) 匹配资源访问控制策略 \ （例如，就已 ACLd claims\ 文件服务器） 或信赖方策略 \ (例如，知道 claims\ Web application\)。 这可以帮助管理员设置资源或应用程序更好地访问控制策略：  
  
    -   仅可以通过 Windows 集成身份验证访问。  
  
    -   可通过广告 FS 身份验证机制访问 Internet。 广告 FS 可用于转换广告 DS 插入可以封装为 SAML 标记，这可能由 Internet 访问资源或信赖方应用程序的广告 FS 声明颁发设备索赔。  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>广告 DS 和广告 FS 之间的差异颁发索赔  
有两个区分因素很重要，若要了解有关广告 DS 与广告 FS 颁发的索赔。 这些差异，包括：  
  
-   广告 DS 只能颁发程序封装在 Kerberos 票证，不 SAML 标记的索赔。 有关如何广告 DS 问题索赔的详细信息，请参阅[动态访问控制内容路线图](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
-   广告 FS 只能颁发封装在 SAML 标记，而不是 Kerberos 门票的索赔。 有关如何广告 FS 问题索赔的详细信息，请参阅[的索赔引擎角色](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)。  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>广告 DS 颁发索赔与广告 FS 相关的工作方式  
广告 DS 颁发索赔可与广告 FS 访问直接从该用户的身份验证的上下文，而不是拨打电话单独 LDAP 去 Active Directory 的用户和设备的索赔。 下图和相应步骤讨论此过程中启用 claims\ 基于访问控制动态访问控制情景的更多详细信息的工作方式。  
  
![使用声明](media/UsingADDSClaimswithADFS.gif)  
  
1.  广告 DS 管理员广告 DS 方案中可使用 Active Directory 管理中心主机或 PowerShell cmdlet 到启用索赔特定类型的对象。  
  
2.  广告 FS 管理员用于广告 FS 管理控制台创建和配置索赔提供商和依赖方有信任 pass\ 通过或转换声称规则。  
  
3.  Windows 客户端尝试访问该网络。 作为 Kerberos 身份验证过程的一部分，的客户端将提供其用户还计算机 ticket\ 授予票证 \(TGT\) 这不包含任何索赔，域控制器。 然后，域控制器查找广告 DS 启用的索赔类型，并包含 Kerberos 票证返回任何结果的索赔。  
  
4.  当尝试访问 ACLd 要求索赔，文件资源 user\ 客户时，他们可以访问资源，因为已从 Kerberos 揭示复合 ID 具有这些索赔。  
  
5.  相同的客户端尝试访问配置为广告 FS 认证的网站或 Web 应用程序，用户将重定向到了 Windows 的集成身份验证配置广告 FS 联盟服务器。 客户端发送到使用 Kerberos 域控制器的请求。 域控制器颁发 Kerberos 票证包含请求的索赔的客户端以提供给联合身份验证的服务器。  
  
6.  基于索赔提供商配置了索赔规则的方式和依赖方信任，以前配置管理员，广告从 Kerberos 票证读取索赔和对该问题的客户端 SAML 令牌包含它们。  
  
7.  客户端接收 SAML 标记包含正确的索赔，然后重定向到该网站。  
  
有关如何创建索赔规则广告 DS 颁发声称处理广告 FS 所需的详细信息，请参阅[创建规则转换传入声称](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
