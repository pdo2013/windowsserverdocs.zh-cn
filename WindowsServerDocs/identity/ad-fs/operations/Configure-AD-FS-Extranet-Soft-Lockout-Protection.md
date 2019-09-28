---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 配置 AD FS Extranet 锁定保护
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bb5958f8205271fe3ab2258ed9812ae03f2a0be0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358208"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>配置 AD FS Extranet 锁定保护

在 Windows Server 2012 R2 的 AD FS 中，我们引入了一种称为 Extranet 锁定的安全功能。  利用此功能，AD FS 会在一段时间内 "停止" 向外部 "恶意" 用户帐户进行身份验证。  这可以防止用户帐户在 Active Directory 中被锁定。  除了保护用户免受 AD 帐户锁定以外，AD FS extranet 锁定还可防止暴力破解密码猜测攻击

> [!NOTE]
> 此功能仅适用于**extranet 方案**，其中，身份验证请求通过 Web 应用程序代理，并且仅适用于**用户名和密码身份验证**。

## <a name="advantages-of-extranet-lockout"></a>Extranet 锁定的优点
Extranet 锁定提供以下主要优势：
- 它可以防止用户帐户遭受**暴力破解攻击**，攻击者会尝试通过持续发送身份验证请求来猜测用户的密码。 在这种情况下，AD FS 会锁定恶意用户帐户，以进行 extranet 访问
- 它保护用户帐户免遭**恶意帐户锁定**，攻击者需要通过发送具有错误密码的身份验证请求来锁定用户帐户。 在这种情况下，尽管用户帐户将被 AD FS 用于 extranet 访问，但 AD 中的实际用户帐户未锁定，用户仍可访问组织内的公司资源。 这称为**软锁定**。

## <a name="how-it-works"></a>工作原理
AD FS 中有3个设置，你需要配置这些设置才能启用此功能： 
- **EnableExtranetLockout &lt;Boolean @ no__t-2**如果要启用 Extranet 锁定，请将此布尔值设置为 True。
- **ExtranetLockoutThreshold &lt;Integer @ no__t-2**定义错误密码尝试的最大数目。 达到阈值后，AD FS 将立即拒绝来自 extranet 的请求，而不会尝试联系域控制器进行身份验证，不管密码是否良好或是否正确都是如此。 这意味着，当帐户被软锁定时，AD 帐户的**badPwdCount**属性的值将不会增加。
- **ExtranetObservationWindow &lt;TimeSpan @ no__t-2**这确定将用户帐户软锁定的时间长度。当传递窗口时，AD FS 将开始再次执行用户名和密码身份验证。 AD FS 使用 AD 特性 badPasswordTime 作为参考来确定 extranet 观察窗口是否已通过。 如果当前时间 > badPasswordTime + ExtranetObservationWindow，则窗口已通过。 

> [!NOTE]
> 独立于 AD 锁定策略 AD FS extranet 锁定功能。 但是，我们强烈建议你将**ExtranetLockoutThreshold**参数值设置为小于 AD 帐户锁定阈值的值。 否则，会导致 AD FS 无法保护在 Active Directory 中锁定的帐户。 

使用最多15个错误密码尝试和30分钟软锁定持续时间的启用 Extranet 锁定功能的示例如下所示：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

这些设置将应用于 AD FS 服务可以进行身份验证的所有域。 它的工作方式是，在 AD FS 收到身份验证请求时，它将通过 LDAP 调用来访问主域控制器（PDC），并针对 PDC 上的用户执行**badPwdCount**属性查找。 如果 AD FS 查找**badPwdCount** > = ExtranetLockoutThreshold 设置的值，并且尚未传递 Extranet 观察窗口中定义的时间，AD FS 将立即拒绝请求，这意味着无论用户是否进入良好或不良来自 extranet 的密码，登录将失败，因为 AD FS 不会将凭据发送到 AD。 AD FS 不会维护**badPwdCount**或锁定用户帐户相关的任何状态。 AD FS 使用 AD 进行所有状态跟踪。 

> [!warning]
> 如果已启用 Server 2012 R2 上 AD FS Extranet 锁定，则通过 WAP 的 AD FS 验证所有通过 WAP 的身份验证请求。 PDC 不可用时，用户将无法从 extranet 进行身份验证。

Server 2016 提供了一个额外的参数，该参数允许 AD FS 在 PDC 不可用时回退到另一个域控制器：

- **ExtranetLockoutRequirePDC &lt;Boolean @ no__t** -启用后： extranet 锁定需要主域控制器（PDC）。 禁用时： extranet 锁定将回退到另一域控制器，以防 PDC 不可用。

你可以使用以下 Windows PowerShell 命令在服务器2016上配置 AD FS extranet 锁定：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>使用 Active Directory 锁定策略
AD FS 中的 Extranet 锁定功能独立于 AD 锁定策略。 但是，您确实需要确保 Extranet 锁定的设置已正确配置，以便它能够为其安全目的提供 AD 锁定策略。
首先，让我们看一看 AD 锁定策略。 AD 中有三个关于锁定策略的设置：
- **帐户锁定阈值**：此设置类似于 AD FS 中的 ExtranetLockoutThreshold 设置。 它确定将导致用户帐户被锁定的登录尝试失败次数。为了保护用户帐户免遭恶意帐户锁定攻击，你需要在 AD FS 中将 ExtranetLockoutThreshold 的值设置 @no__t AD 中的帐户锁定阈值
- **帐户锁定持续时间**：此设置确定用户帐户被锁定的时间长度。此设置在此对话中并不重要，因为 Extranet 锁定应始终发生，然后进行 AD 锁定（如果配置正确）
- **重置帐户锁定计数器**：此设置确定在**badPwdCount**重置为0之前，用户上次登录失败所需的时间。 若要在 AD FS 中使用 Extranet 锁定功能，请确保在 ad 中的值后面的 "重置帐户锁定计数器" AD FS &gt; 中的 ExtranetObservationWindow 值。 下面的示例说明了原因。  

让我们看看两个示例，并了解**badPwdCount**如何基于不同的设置和状态随时间而变化。 假设两个示例**帐户锁定阈值**= 4， **ExtranetLockoutThreshold** = 2。 **红色**箭头表示密码尝试错误，**绿色**箭头表示正确的密码尝试。 例如 #1， **ExtranetObservationWindow** &gt;**重置帐户锁定计数器**。 例如 #2， **ExtranetObservationWindow** &lt;**重置帐户锁定计数器**。 

### <a name="example-1"></a>示例 1
![示例1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>示例 2
![示例1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

如上图所示， **badPwdCount**将重置为0时，有两个条件。 一个是成功登录时。 另一种情况是在设置**后重置 "重置帐户锁定计数器**" 中定义的计数器。 如果在 &lt; **ExtranetObservationWindow** **后重置帐户锁定计数器**，帐户将不会有任何被 AD 锁定的风险。 但是，如果在 &gt; **ExtranetObservationWindow** **后重置帐户锁定计数器**，则可能是因为 AD 锁定了帐户，但该帐户已被 "延迟"。 可能需要一段时间才能获取 AD 锁定的帐户，具体取决于你的配置，因为 AD FS 在**badPwdCount**达到**帐户锁定阈值**之前，将只允许一次错误密码尝试。

有关详细信息，请参阅[配置帐户锁定](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/)。 

## <a name="known-issues"></a>已知问题
存在一个已知问题，即 AD 用户帐户无法使用 AD FS 进行身份验证，因为**badPwdCount**属性不会复制到 ADFS 正在查询的域控制器。 有关更多详细信息，请参阅[2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc) 。 可在[此处](../deployment/updates-for-active-directory-federation-services-ad-fs.md)找到到目前为止发布的所有 AD FS qfe。

## <a name="key-points-to-remember"></a>需要记住的要点
- Extranet 锁定功能仅适用于身份验证请求通过 Web 应用程序代理的**extranet 方案**
- Extranet 锁定功能仅适用于**username & 密码身份验证**
- AD FS 不会跟踪已软锁定的**badPwdCount**或用户的任何跟踪。AD FS 使用 AD 进行所有状态跟踪
- AD FS 通过在 PDC 上针对每次身份验证尝试的用户 LDAP 调用来执行**badPwdCount**属性的查找  
- 如果 AD FS 早于2016的版本无法访问 PDC，将会失败。 AD FS 2016 引入了一些改进功能，在 PDC 不可用时，它们将允许 AD FS 回退到其他域控制器。 
- 如果 badPwdCount < ExtranetLockoutThreshold，AD FS 将允许来自 extranet 的身份验证请求 
- 如果**badPwdCount** >= **ExtranetLockoutThreshold** AND **badPasswordTime** + **ExtranetObservationWindow** < 当前时间，则 AD FS 将拒绝来自 extranet 的身份验证请求
- 若要避免恶意帐户锁定，应确保**ExtranetLockoutThreshold** < **帐户锁定阈值**和**ExtranetObservationWindow** > **重置帐户锁定计数器**


## <a name="additional-references"></a>其他参考  
- [保护 Active Directory 联合身份验证服务的最佳实践](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [向非管理员用户委派 AD FS Powershell Commandlet 访问权限](delegate-ad-fs-pshell-access.md)
- [Set-adfsproperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

    
