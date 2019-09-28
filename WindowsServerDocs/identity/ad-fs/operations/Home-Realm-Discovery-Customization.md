---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: 主领域发现自定义
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5a151e46e566d9f5459419771cbd476bb26c248d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357961"
---
# <a name="home-realm-discovery-customization"></a>主领域发现自定义


当 AD FS 客户端第一次请求资源时，资源联合服务器不包含有关客户端领域的任何信息。 资源联合服务器使用 "**客户端领域发现**" 页响应 AD FS 客户端，其中用户从列表中选择主领域。 列表的值由“声明提供方信任”中的显示名称属性填充。 使用以下 Windows PowerShell cmdlet 来修改和自定义 AD FS 主领域发现体验。  
  
![主领域](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> 请注意为本地 Active Directory 显示的“声明提供方”名称是联合身份验证服务显示名称。  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>配置身份提供程序以使用某些电子邮件后缀  
组织可以与多个声明提供方联合。 AD FS 现在为管理员提供了 @ no__t-0box 功能，使管理员能够列出声明提供程序支持的后缀 @no__t @no__t，并为后缀 @ no__t 3based 发现启用此后缀。 通过此配置，最终用户可以键入其组织帐户，AD FS 会自动选择相应的声明提供程序。  
  
若要配置标识提供者 \(IDP @ no__t，如 `fabrikam`，使用某些电子邮件后缀，请使用以下 Windows PowerShell cmdlet 和语法。  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> 在两台 AD FS 服务器之间进行联合时，请将声明提供程序信任上的 PromptLoginFederation 属性设置为 ForwardPromptAndHintsOverWsFederation。  这是为了 AD FS 会将 login_hint 和 prompt 参数转发到 IDP。  可以通过运行以下 PowerShell cmdlet 来完成此操作：
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>按照信赖方配置身份提供程序列表  
在某些情况下，组织可能希望最终用户仅看到特定于应用程序的声明提供方，以便只有一个声明提供方子集显示在主领域发现页面上。  
  
若要为每个信赖方 \(RP @ no__t 配置 IDP 列表，请使用以下 Windows PowerShell cmdlet 和语法。  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>绕过 Intranet 的主领域发现  
大多数组织仅支持任何用户都从其防火墙内部访问其本地 Active Directory。 在这些情况下，管理员可以将 AD FS 配置为绕过 intranet 的主领域发现。  
  
若要绕过 intranet 的 HRD，请使用以下 Windows PowerShell cmdlet 和语法。  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> 请注意，如果已配置信赖方的标识提供程序列表，即使已启用上一个设置并且用户从 intranet 进行访问，AD FS 仍会显示主领域发现 \(HRD @ no__t 页。 若要在这种情况下绕过 HRD，你必须确保也将“Active Directory”添加到此信赖方的 IDP 列表。  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
