---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: "家庭领域发现自定义"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 328c313b2d5bf174f6e6af20cd91ac9620edac82
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="home-realm-discovery-customization"></a>家庭领域发现自定义

>适用于：Windows Server 2016，Windows Server 2012 R2

当广告 FS 客户端首先请求资源时，资源联盟服务器具有领域客户端的任何信息。 资源联盟服务器响应广告 FS 客户端与**客户端领域发现**页上，在用户可以从列表中选择家庭的领域。 列表值填充的索赔提供商信任中的显示名称属性。 使用下面的 Windows PowerShell cmdlet 修改和自定义家庭广告 FS 领域发现体验。  
  
![家庭的领域](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> 请注意显示本地 Active Directory 的索赔提供程序名称联合身份验证服务显示名称。  
  
## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>配置使用某些电子邮件后缀身份提供程序  
组织可以联盟具有多个索赔提供商。 广告 FS 现在提供适用于管理员列出后缀，例如，in\ 框功能@us.contoso.com， @eu.contoso.com、，它是受支持的索赔提供商而为 suffix\ 基于发现启用。 使用该配置，最终用户可以在他们组织的帐户中键入，并广告 FS 自动选择相应的索赔提供程序。  
  
若要配置身份提供 \(IDP\)，如`fabrikam`来使用某些电子邮件后缀，使用以下 Windows PowerShell cmdlet 和语法。  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
  
## <a name="configure-an-identity-provider-list-per-relying-party"></a>配置依赖每身份提供商列表方  
在某些情况下，组织可能需要最终用户仅查看索赔提供商，以便子集索赔提供商的副本显示主页领域发现页上的特定于应用程序。  
  
若要配置每依赖 IDP 列表方 \(RP\)，使用以下 Windows PowerShell cmdlet 和语法。  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>绕过家庭领域发现的 intranet  
大多数组织仅支持从其防火墙内访问的任何用户其当地的 Active Directory。 在这些情况下，管理员可以配置广告 FS 绕过的 intranet 主页领域发现。  
  
要避开的 intranet HRD，使用以下 Windows PowerShell cmdlet 和语法。  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> 请注意，如果依赖的身份提供商列表方已配置，即使屏幕已启用之前的设置，从 intranet 用户访问广告 FS 仍然显示主页领域发现 \(HRD\) 页。 在此情况下跳过 HRD，你必须确保"Active Directory"还添加到 IDP 列表为该信赖方。  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  
