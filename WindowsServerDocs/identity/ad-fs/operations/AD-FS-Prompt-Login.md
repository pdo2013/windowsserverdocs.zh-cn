---
title: AD FS 提示符 = 登录名
description: 为 AD FS 2016 的常见问题
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6a4b6cfe98064181824e210be9031a0f67cb4b75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824628"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory 联合身份验证服务提示 = 登录名参数支持
以下文档介绍了 AD FS 中提供 prompt = login 参数的本机支持。

## <a name="what-is-promptlogin"></a>什么是 prompt = 登录名？  

某些 Office 365 应用程序 （与启用新式身份验证） 发送到每个身份验证请求的一部分的 Azure AD prompt = login 参数。  默认情况下，Azure AD 将其转变成两个参数： <code> <b> wauth </b> =urn:oasis:names:tc:SAML:1.0:am:password </code>，以及<code> <b> wfresh </b> =0 </code>.

这会导致企业 intranet 和多重身份验证方案需要用户名和密码以外的身份验证类型，则在其中的问题。  

AD FS 2016 年 7 月更新汇总的 Windows Server 2012 R2 中引入了对 prompt = login 参数的本机支持。  这意味着，现在可以选择配置 Azure AD 要发送此参数为的是 AD FS 服务一部分的 Azure AD 和 Office 365 身份验证请求。

### <a name="ad-fs-versions-that-support-promptlogin"></a>支持提示符的 AD FS 版本 = 登录名
下面是支持 prompt = login 参数的 AD FS 版本的列表。

- AD FS 内使用 2016 年 7 月的 Windows Server 2012 R2 更新汇总

- Windows Server 2016 中的 AD FS

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>如何配置 Azure AD 租户发送提示 = 登录到 AD FS

使用 Azure AD PowerShell 模块配置设置。

> [!NOTE]
> （已启用由 PromptLoginBehavior 属性） 的提示 = 登录名功能是仅在当前可用[版本 1.0 Azure AD Powershell 模块](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)，cmdlet 在其中使用包括"Msol"，如下所述的名称Set-msoldomainfederationsettings。  它不是当前可通过版本 2.0 Azure AD PowerShell 模块，其 cmdlet 具有名称如"集 AzureAD\*"。

若要配置提示符 = 登录名的行为，下面的 cmdlet 语法：

示例 1：
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

示例 2：
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

示例 3：
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 PreferredAuthenticationProtocol、 SupportsMfa 和 PromptLoginBehavior 属性值可通过查看该 cmdlet 的输出：![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> 默认情况下，运行 Get-msoldomainfederationsettings 时，某些属性不会显示在控制台中。  若要查看建议你使用这些参数 |fl * 若要强制所有对象的属性的输出。


以下是有关 PromptLoginBehavior 参数及其设置的详细信息。
   
   - <b>TranslateToFreshPasswordAuth</b>意味着发送的默认 Azure AD 行为<b>wauth</b>并<b>wfresh</b>到 AD FS 而不是提示 = 登录名
   - <b>NativeSupport</b> prompt = login 参数将按顺序发送到 AD FS 的方式
   - <b>已禁用</b>意味着执行任何操作将发送到 AD FS

