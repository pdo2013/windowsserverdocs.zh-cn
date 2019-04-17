---
title: "广告 FS 提示 = 登录"
description: "对于广告 FS 2016：常见问题"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8b98cdc27c9bd01d9855e98965f59ebe5257ed5c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory 联合身份验证服务提示 = 登录参数支持
下面的文档描述适用于广告 FS 的提示 = 登录参数的本机支持。

## <a name="what-is-promptlogin"></a>什么是提示 = 登录？  

某些 Office 365 的应用程序（现代身份验证已启用）将发送到 Azure AD 作为身份验证的每个请求的一部分的提示 = 登录参数。  默认情况下，Azure AD 转化这两个参数：<code><b>wauth</b>=urn:oasis:names:tc:SAML:1.0:am:password</code>，并<code><b>wfresh</b>=0</code>。

这可能导致公司的 intranet 和多重身份验证在其中所需身份验证类型以外的用户名和密码的情况下的问题。  

在 2016 年 7 月更新汇总与 Windows Server 2012 R2 的广告 FS 引入提示 = 登录参数的本机支持。  这意味着你现在可以选择配置 Azure AD 发送此参数作为-作为 Azure AD 和 Office 365 身份验证的请求是广告 FS 服务。

### <a name="ad-fs-versions-that-support-promptlogin"></a>广告 FS 版本支持提示 = 登录
以下是广告 FS 版本支持的提示 = 登录参数列表。

- 在 2016 年 7 月 Windows Server 2012 R2 的广告 FS 更新汇总

- 在 Windows Server 2016 的广告 FS

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>如何配置发送提示你 Azure AD 租户 = 登录到广告 FS

使用 Azure AD PowerShell 模块配置设置。

> [!NOTE]
> （由 PromptLoginBehavior 属性启用）的提示 = 登录的功能是当前仅适用于[' 版本 1.0' Azure AD Powershell 模块](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)，在其中 cmdlet 已包含"Msol"如 Set-MsolDomainFederationSettings 的名称。  它处于当前不可用通过版本 2.0' Azure AD PowerShell 模块，其 cmdlet 具有名称诸如"设置-AzureAD\ *"。

若要配置提示符 = 登录行为，以下 cmdlet 语法：

示例 1:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

示例 2:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

示例 3:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 可以通过查看 cmdlet 的输出中找到 PreferredAuthenticationProtocol、SupportsMfa，以及 PromptLoginBehavior 属性值：![获取 MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> 默认情况下，运行 Get-MsolDomainFederationSettings 时，某些属性不会显示在该控制台。  若要查看这些建议你使用的参数 |佛罗里达 * 强制输出中的所有的对象属性。


下面是有关 PromptLoginBehavior 参数及其设置的详细信息。
   
   - <b>TranslateToFreshPasswordAuth</b>意味着发送的默认 Azure AD 行为<b>wauth</b>和<b>wfresh</b>到广告 FS，而不是提示符 = 登录
   - <b>NativeSupport</b>提示 = 登录参数将广告 FS 原样发送的方式
   - <b>禁用</b>意味着执行任何操作将发送到广告 FS

