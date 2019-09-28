---
title: AD FS prompt = login
description: AD FS 2016 的常见问题
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ba43d591d429e589a990dfdfc4a4992ffa4a6ef5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358546"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory 联合身份验证服务 prompt = 登录参数支持

以下文档介绍 AD FS 中提供的 prompt = login 参数的本机支持。

## <a name="what-is-promptlogin"></a>什么是 prompt = login？

当应用程序需要请求 Azure AD 的全新身份验证时，这意味着他们需要 Azure AD 重新对用户进行身份验证，即使用户已进行身份验证，他们也可以`prompt=login`在身份验证过程中将参数发送到 Azure AD需要.

当此请求用于联合用户时，Azure AD 需要通知 IdP （如 AD FS），请求是进行全新身份验证。

默认情况下，Azure AD `prompt=login`转换`wfresh=0`为`wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` ，并在将此类身份验证请求发送到联合 IdP 时使用。

这些参数意味着：

- `wfresh=0`：执行全新身份验证
- `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`：使用用户名/密码执行全新身份验证请求

这可能会导致公司 intranet 和多重身份验证方案出现问题，在这种情况下，需要使用用户名和密码以外的身份`wauth`验证类型作为参数的要求。  

Windows Server 2012 R2 中包含年 7 2016 月更新汇总的 AD FS 引入了`prompt=login`对参数的本机支持。 这意味着现在 Azure AD 可以按原样发送此参数以作为 Azure AD 和 Office 365 身份验证请求中的 AD FS 服务。

## <a name="ad-fs-versions-that-support-promptlogin"></a>支持 prompt = login 的 AD FS 版本

下面列出了支持`prompt=login`参数的 AD FS 版本。

- 2012 2016 年7月更新汇总的 Windows Server R2 中的 AD FS
- Windows Server 2016 中的 AD FS

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>如何配置联合域以发送 prompt = 登录到 AD FS

使用 Azure AD PowerShell 模块来配置设置。

> [!NOTE]
> 此功能（通过属性启用）当前仅在[Azure AD Powershell 模块版本 1.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)中提供，其中 cmdlet 包含包含 "Msol" 的名称，如 set-msoldomainfederationsettings。 `PromptLoginBehavior` `prompt=login`  它当前不可通过 Azure AD PowerShell 模块的 "2.0 版" 提供，其 cmdlet 的名称类似于 "AzureAD\*"。

1. 首先`PreferredAuthenticationProtocol`，通过运行以下 PowerShell 命令`SupportsMfa`获取联合`PromptLoginBehavior`域的、和的当前值：

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> 默认`Get-MsolDomainFederationSettings`情况下，的输出不会在控制台中显示某些属性。 若要查看所有属性，应通过管道`|`（）将其`Format-List *`输出发送到，以强制对象的所有属性的输出。

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> 如果属性为空（`$null`），则`TranslateToFreshPasswordAuth`表示的默认行为。 `PreferredAuthenticationMethod`

2. `PromptLoginBehavior`通过运行以下命令来配置所需的值：

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

以下是`PromptLoginBehavior`参数的可能值及其含义：

- **TranslateToFreshPasswordAuth**：表示转换`prompt=login`为`wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`和`wfresh=0`的默认 Azure AD 行为。
- **NativeSupport**：表示`prompt=login`参数将按原样发送到 AD FS。 如果 AD FS 在 Windows Server 2012 R2 中且具有7月2016更新汇总或更高版本，则这是建议的值。
- **Disabled**：表示仅`wfresh=0`将发送到 AD FS。
