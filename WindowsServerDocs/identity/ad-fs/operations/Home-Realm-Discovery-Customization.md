---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: 主领域发现自定义项
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1198d8b76f2ecdad728e2de6ce7a5c0d053f779f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868928"
---
# <a name="home-realm-discovery-customization"></a>主领域发现自定义项

>适用于：Windows Server 2016, Windows Server 2012 R2

当 AD FS 客户端首次请求资源时，资源联合身份验证服务器具有有关领域的客户端的任何信息。 资源联合身份验证服务器与 AD FS 客户端响应**客户端领域发现**页面上，其中用户从列表选择主领域。 列表的值由“声明提供方信任”中的显示名称属性填充。 使用以下 Windows PowerShell cmdlet 来修改和自定义主 AD FS 领域发现体验。  
  
![主领域](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> 请注意为本地 Active Directory 显示的“声明提供方”名称是联合身份验证服务显示名称。  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>配置身份提供程序以使用某些电子邮件后缀  
组织可以与多个声明提供方联合。 AD FS 现在提供了在\-框来列出后缀，例如，管理员的功能@us.contoso.com， @eu.contoso.com，即受声明提供程序并启用该后缀\-基于发现。 使用此配置，最终用户可以键入其组织帐户和 AD FS 会自动选择相应的声明提供程序。  
  
若要配置的标识提供者\(IDP\)，如`fabrikam`中要使用某些电子邮件后缀，请使用以下 Windows PowerShell cmdlet 和语法。  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> 联合两个 AD FS 服务器之间时, 将 PromptLoginFederation 属性设置为 ForwardPromptAndHintsOverWsFederation 声明提供方信任上。  这是，使 AD FS 将将 login_hint 和提示参数转发到 IDP。  这可以通过运行以下 PowerShell cmdlet:
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>按照信赖方配置身份提供程序列表  
在某些情况下，组织可能希望最终用户仅看到特定于应用程序的声明提供方，以便只有一个声明提供方子集显示在主领域发现页面上。  
  
若要配置 IDP 列表按信赖方\(RP\)，使用以下 Windows PowerShell cmdlet 和语法。  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>绕过 Intranet 的主领域发现  
大多数组织仅支持任何用户都从其防火墙内部访问其本地 Active Directory。 在这些情况下，管理员可以配置 AD FS 来绕过 intranet 的主领域发现。  
  
若要绕过 intranet 的 HRD，请使用以下 Windows PowerShell cmdlet 和语法。  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> 请注意，是否已配置为信赖方的标识提供程序列表，即使以前的设置已启用并且用户从 intranet 进行访问，AD FS 仍显示主领域发现\(HRD\)页。 若要在这种情况下绕过 HRD，你必须确保也将“Active Directory”添加到此信赖方的 IDP 列表。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
