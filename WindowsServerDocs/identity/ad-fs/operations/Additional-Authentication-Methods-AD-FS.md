---
title: AD FS 2019 中的其他身份验证方法
description: 本文档介绍在 AD FS 2019 中新的身份验证方法。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 68cd67dc14d3407985579a49e2f8603634fafdb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824428"
---
# <a name="configure-3rd-party-authenticaiton-providers-as-primary-authentication-in-ad-fs-2019"></a>将第三方身份验证提供程序配置为 AD FS 2019 中的主要身份验证

>适用于：Windows Server 2019

组织遇到尝试暴力破解攻击、 泄漏，或否则锁定用户帐户通过发送基于密码的身份验证请求。  为了帮助保护组织免受损坏，AD FS 引入了功能，例如"智能"extranet 锁定和阻止 IP 地址为基础。  

但是，这些缓解措施是被动的。  若要提供主动的方法，以减少这些攻击的严重性 AD FS 具有非密码因素之前收集密码提示的功能。  

例如，AD FS 2016 引入了 Azure MFA 作为主要身份验证，以便从 Authenticator 应用的 OTP 代码可用作第一个因素。
使用 AD FS 2019 生成对此，可以将外部身份验证提供程序配置为主要身份验证因素。

有两个这样的方案：

## <a name="scenario-1-protect-the-password"></a>方案 1： 保护密码
通过首先提示输入一个附加的外部因素保护免受暴力破解攻击和锁定的基于密码的登录名。  仅当已成功完成的外部身份验证没有用户然后看到提示输入密码。  这消除了攻击者尝试破坏或禁用的帐户的简便方法。

此方案包含两个组件：
- 提示输入 Azure MFA 或外部身份验证身份作为主要身份验证
- 用户名和密码作为 AD FS 中的附加身份验证

## <a name="scenario-2-password-free"></a>方案 2： 无密码的 ！
完全消除密码，但完成强，使用完全非密码的多重身份验证基于 AD FS 中的方法
- Azure MFA 的身份验证器应用
- Windows 10 Hello 企业版
- 证书身份验证
- 外部身份验证提供程序

## <a name="concepts"></a>概念
什么**主要身份验证**方法实际上是，它是第一个，其他因素之前提示用户的方法。  以前只有主方法中的 AD FS 提供了生成的方法中为 Active Directory 或 Azure MFA 或其他 LDAP 身份验证存储。  外部方法无法配置为"其他"身份验证，这在主要身份验证已成功完成后发生。

在 AD FS 2019 中，为主要功能的外部身份验证意味着在 AD FS 服务器场 （使用 Register-adfsauthenticationprovider） 中注册任何外部身份验证提供程序变得可用于主要身份验证，以及"其他"身份验证。 它们可以启用内置提供者，窗体身份验证和证书身份验证用于 intranet 和/或 extranet 使用方式相同。

![身份验证](media/Additional-Authentication-Methods-AD-FS/auth1.png)

一旦为 extranet，intranet 上，启用了外部提供程序和 / 或变得可供用户使用。  如果启用了多个方法，用户将看到一个页面，选择并能够选择主方法，与它们进行其他身份验证。

## <a name="pre-requisites"></a>先决条件
作为主配置外部身份验证提供程序前, 请确保满足以下先决条件已事先
- AD FS 场行为级别 (FBL) 引发了为"4"（此值会转换为 AD FS 2019）
    - 这是新的 AD FS 2019 场的默认 FBL 值
    - 对于 AD FS 基于 Windows Server 2012 R2 或 2016年场，以 FBL 都可以使用 PowerShell commandlet Invoke AdfsFarmBehaviorLevelRaise 引发。  有关升级 AD FS 场的详细信息，请参阅升级一文，了解 SQL 场或 WID 场的场 
    - 你可以检查使用 cmdlet 获取 AdfsFarmInformation FBL 值
- AD FS 2019 场配置为使用新面向页面的 2019年分页用户
    - 这是新的 AD FS 2019 场的默认行为
    - 对于从 Windows Server 2012 R2 或 2016年升级 AD FS 场，分页的流时将自动启用 （在本文档介绍该功能） 已启用为主要的外部身份验证，如下所述。

## <a name="enable-external-authentication-methods-as-primary"></a>主数据库启用外部身份验证方法
确认系统必备组件后，有两种方法配置为主 AD FS 附加身份验证提供程序：

### <a name="using-powershell"></a>使用 PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


启用或禁用主作为附加身份验证后，必须重新启动 AD FS 服务。

### <a name="using-the-ad-fs-management-console"></a>使用 AD FS 管理控制台
在 AD FS 管理控制台中下,**服务** -> **身份验证方法**下**主要身份验证方法**，单击编辑

单击复选框**允许其他身份验证提供程序作为主**。

启用或禁用主作为附加身份验证后，必须重新启动 AD FS 服务。

## <a name="enable-username-and-password-as-additional-authentication"></a>启用作为附加身份验证的用户名和密码
若要完成的"保护密码"的方案，启用用户名和密码作为附加身份验证使用 PowerShell 或 AD FS 管理控制台
### <a name="using-powershell"></a>使用 PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>使用 AD FS 管理控制台
在 AD FS 管理控制台中下,**服务** -> **身份验证方法**下**其他身份验证方法**，单击**编辑**

单击复选框**窗体身份验证**启用用户名和密码作为附加身份验证。
