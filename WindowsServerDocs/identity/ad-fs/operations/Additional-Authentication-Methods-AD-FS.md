---
title: AD FS 2019 中的其他身份验证方法
description: 本文档介绍 AD FS 2019 中的新身份验证方法。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1800ccf1cc5c25124887b2d2fa7b76ee68788ef3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866016"
---
# <a name="configure-3rd-party-authentication-providers-as-primary-authentication-in-ad-fs-2019"></a>将第三方身份验证提供程序配置为 AD FS 2019 中的主要身份验证


组织遇到了攻击，这些攻击通过发送基于密码的身份验证请求尝试暴力破解、泄露或以其他方式锁定用户帐户。  为了帮助保护组织免受危害，AD FS 引入了一些功能，例如 extranet "智能" 锁定和基于 IP 地址的阻止。  

不过，这些缓解措施是被动的。  若要提供主动的方式，以便降低这些攻击的严重性，AD FS 可以在收集密码之前提示输入非密码因素。  

例如，AD FS 2016 将 Azure MFA 引入为主要身份验证，以便可以将验证器应用中的 OTP 代码用作第一个因素。
基于这一点，通过 AD FS 2019，你可以将外部身份验证提供程序配置为主要身份验证因素。

这会启用两种主要方案：

## <a name="scenario-1-protect-the-password"></a>方案1：保护密码
首先提示额外的外部因素，以防止基于密码的登录免受暴力攻击和锁定。  仅当外部身份验证成功完成后，用户会看到密码提示符。  这消除了攻击者尝试破坏或禁用帐户的一种简便方法。

此方案由以下两个组件组成：
- 作为主要身份验证的 Azure MFA 或外部身份验证因素提示
- 作为 AD FS 中附加身份验证的用户名和密码

## <a name="scenario-2-password-free"></a>方案2：密码免费！
完全消除密码，但使用 AD FS 中的完全非密码方法完成强多重身份验证
- Azure MFA 与验证器应用
- Windows 10 Hello 企业版
- 证书身份验证
- 外部身份验证提供程序

## <a name="concepts"></a>概念
**主要的身份验证**是指在其他因素之前提示用户首先出现的方法。  以前，AD FS 中唯一可用的主要方法是内置 Active Directory 或 Azure MFA 或其他 LDAP 身份验证存储的方法。  外部方法可以配置为 "附加" 身份验证，这是在主身份验证成功完成后发生的。

在 AD FS 2019 中，作为主要功能的外部身份验证意味着在 AD FS 场中注册的任何外部身份验证提供程序（使用 Register-adfsauthenticationprovider）都可用于主要身份验证和 "其他"验证. 可以采用与内置提供程序（如 Forms 身份验证和证书身份验证）相同的方式来启用这些功能，以用于 intranet 和/或 extranet。

![身份验证](media/Additional-Authentication-Methods-AD-FS/auth1.png)

为 extranet、intranet 或这两者启用外部提供程序后，可供用户使用。  如果启用了多个方法，则用户将看到一个 "选择" 页，并且可以选择一个主要方法，就像对其他身份验证一样。

## <a name="pre-requisites"></a>先决条件
在将外部身份验证提供程序配置为主要身份验证提供程序之前，请确保满足以下先决条件：
- 已将 AD FS 场行为级别（FBL）提升为 "4" （此值转换为 AD FS 2019）
    - 这是新 AD FS 2019 场的默认 FBL 值
    - 对于基于 Windows Server 2012 R2 或2016的 AD FS 场，可以使用 PowerShell commandlet AdfsFarmBehaviorLevelRaise 引发 FBL。  有关升级 AD FS 场的详细信息，请参阅 SQL 场或 WID 场的场升级一文 
    - 可以使用 cmdlet AdfsFarmInformation 来检查 FBL 值
- AD FS 2019 场配置为使用新的2019页面面向用户页面
    - 这是新 AD FS 2019 场的默认行为
    - 对于从 Windows Server 2012 R2 或2016升级的 AD FS 场，按如下所述启用了外部身份验证（本文档所述的功能）时，将自动启用分页的流。

## <a name="enable-external-authentication-methods-as-primary"></a>启用外部身份验证方法作为主要身份验证方法
验证先决条件后，可通过两种方式将其他身份验证提供程序配置为主要 AD FS：

### <a name="using-powershell"></a>使用 PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


启用或禁用附加身份验证作为主要身份验证后，必须重新启动 AD FS 服务。

### <a name="using-the-ad-fs-management-console"></a>使用 AD FS 管理控制台
在 AD FS 管理控制台中，在 "**服务** -> **身份验证方法**" 下的 "**主要身份验证方法**" 下，单击 "编辑"

单击 "**允许其他身份验证提供程序作为主要身份验证**" 复选框。

启用或禁用附加身份验证作为主要身份验证后，必须重新启动 AD FS 服务。

## <a name="enable-username-and-password-as-additional-authentication"></a>启用用户名和密码作为附加身份验证
若要完成 "保护密码" 方案，请使用 PowerShell 或 AD FS 管理控制台将用户名和密码作为附加身份验证启用
### <a name="using-powershell"></a>使用 PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>使用 AD FS 管理控制台
在 AD FS 管理控制台中，在 "**服务** -> **身份验证方法**" 下的 "**其他身份验证方法**" 下，单击 "**编辑**"

单击 "**窗体身份验证**" 复选框可启用用户名和密码作为附加身份验证。
