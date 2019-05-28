---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: 配置 AD FS Extranet 锁定保护
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/01/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6612c05e664b50c5a50b10b712b91715cc85d230
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189882"
---
# <a name="configure-ad-fs-extranet-lockout-protection"></a>配置 AD FS Extranet 锁定保护

在 Windows Server 2012 R2 上的 AD FS，我们引入了名为 Extranet 锁定的安全功能。  使用此功能，AD FS 将"停止"进行身份验证中的"恶意"用户帐户之外的一小段时间。  这会阻止你的用户帐户被锁定在 Active Directory 中。  除了保护用户免受 AD 帐户锁定，AD FS extranet 锁定还可防止暴力密码猜测攻击

> [!NOTE]
> 此功能仅适用于**extranet 方案**在通过 Web 应用程序代理和仅适用于身份验证请求**用户名和密码身份验证**。

## <a name="advantages-of-extranet-lockout"></a>Extranet 锁定的优点
Extranet 锁定提供了以下主要优点：
- 它可以保护中的用户帐户**暴力破解攻击**攻击者尝试通过持续发送身份验证请求来猜测用户的密码。 在这种情况下，AD FS 将锁定为 extranet 访问恶意用户帐户
- 它可以保护中的用户帐户**恶意帐户锁定**攻击者要锁定的用户帐户将使用错误密码的身份验证请求发送。 在这种情况下，尽管将 AD fs 进行 extranet 访问锁定的用户帐户，但 AD 中的实际用户帐户未锁定和用户仍然可以访问组织内的公司资源。 这称为**软锁定**。

## <a name="how-it-works"></a>工作原理
有您需要配置以启用此功能的 AD FS 中的 3 个设置： 
- **EnableExtranetLockout&lt;布尔&gt;** 设置此布尔值为 True，如果你想要启用 Extranet 锁定。
- **ExtranetLockoutThreshold&lt;整数&gt;** 这定义了最大错误密码尝试次数。 达到阈值后，AD FS 会立即拒绝来自 extranet 的请求而不尝试进行身份验证，无论联系域控制器是否密码的好坏，直到传递 extranet 的观察窗口。 这意味着的值**badPwdCount**出软锁定帐户时，将不会增加的 AD 帐户的属性。
- **ExtranetObservationWindow&lt;时间跨度&gt;** 这可确定多长时间的用户帐户将是软-锁定。AD FS 将开始执行用户名和密码身份验证再次传递窗口时。 AD FS 使用 AD 属性 badPasswordTime 作为引用用于确定是否具有已通过 extranet 的观察窗口。 如果当前已通过窗口时间 > badPasswordTime + ExtranetObservationWindow。 

> [!NOTE]
> AD FS extranet 锁定函数独立于 AD 锁定策略。 但是，我们强烈建议您设置**ExtranetLockoutThreshold**为低于 AD 帐户锁定阈值的值的参数值。 如果不这样做将导致无法保护帐户免受被锁定在 Active Directory 中的 AD FS。 

启用最多包含 15 的错误密码尝试次数以及 30 分钟软锁定持续时间的 Extranet 锁定功能的示例如下所示：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30)
```

这些设置将适用于 AD FS 服务可以进行身份验证的所有域。 其工作原理的方法是 AD FS 接收身份验证请求时，它将通过 LDAP 调用访问主域控制器 (PDC) 并执行用于查找**badPwdCount** PDC 上的用户的属性。 如果 AD FS 找到的值**badPwdCount** > = ExtranetLockoutThreshold 设置并将在 Extranet 的观察窗口中定义的时间尚未通过但 AD FS 会拒绝该请求立即，这意味着无论是否用户输入从 extranet 好坏密码，则登录将失败，因为 AD FS 不会发送到 AD 凭据。 AD FS 不维护任何状态与**badPwdCount**或已锁定用户帐户。 AD FS 使用 AD 跟踪的所有状态。 

> [!warning]
> 启用 Server 2012 R2 上的 AD FS Extranet 锁定时通过 WAP 的所有身份验证请求进行验证 PDC 上由 AD FS。 PDC 不可用时，用户将无法从 extranet 进行身份验证。

Server 2016 提供允许 PDC 不可用时回退到另一个域控制器的 AD FS 的其他参数：

- **ExtranetLockoutRequirePDC&lt;布尔&gt;** -时启用： extranet 锁定需要主域控制器 (PDC)。 禁用时： 在 PDC 不可用的情况下，extranet 锁定将回退到另一个域控制器。

可以使用以下 Windows PowerShell 命令在 Server 2016 上配置 AD FS extranet 锁定：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```

## <a name="working-with-the-active-directory-lockout-policy"></a>使用 Active Directory 锁定策略
在 AD FS Extranet 锁定功能独立于 AD 锁定策略。 但是，您确实需要确保正确配置 Extranet 锁定，以便它可以向其安全目的提供 AD 锁定策略设置。
让我们看一看 AD 锁定策略的第一个。 在 AD 中有三个设置有关锁定策略：
- **帐户锁定阈值**： 此设置是类似于 AD FS 中的 ExtranetLockoutThreshold 设置。 它确定将导致用户帐户才会被锁定的失败的登录尝试次数。为了防止恶意帐户锁定攻击你的用户帐户，你想要在 AD FS 设置 ExtranetLockoutThreshold 值&lt;AD 中的帐户锁定阈值值
- **帐户锁定持续时间**： 此设置确定多长时间的用户帐户已锁定。此设置并不重要得多此会话中如果正确配置 AD 锁定之前，应始终发生 Extranet 锁定
- **此后帐户锁定计数器重置**： 此设置确定多长时间从用户的上次登录失败之前必须经过**badPwdCount**重置为 0。 为了在 AD FS Extranet 锁定功能很好地配合 AD 锁定策略，你想要确保 ExtranetObservationWindow 的值中的 AD FS &gt; AD 在此后重置帐户锁定计数器值。 下面的示例将解释原因。  

让我们看一看两个示例，请参阅如何**badPwdCount**随时间根据不同的设置和状态变化。 在这两个示例中假设**帐户锁定阈值**= 4 和**ExtranetLockoutThreshold** = 2。 **红色**箭头表示错误密码尝试**绿色**箭头表示的很好的密码尝试。 示例 #1 中**ExtranetObservationWindow** &gt; **此后重置帐户锁定计数器**。 示例 #2 中**ExtranetObservationWindow** &lt; **此后重置帐户锁定计数器**。 

### <a name="example-1"></a>示例 1
![示例 1](media/Configure-AD-FS-Extranet-Lockout-Protection/one.png)

### <a name="example-2"></a>示例 2
![示例 1](media/Configure-AD-FS-Extranet-Lockout-Protection/two.png)

您可以看到从更高版本，有两个时的条件**badPwdCount**将重置为 0。 一个是成功的登录尝试时。 另一种是在需要时重置此计数器中定义**此后重置帐户锁定计数器**设置。 当**此后重置帐户锁定计数器** &lt; **ExtranetObservationWindow**，一个帐户不具有由 AD 锁定的任何风险。 但是，如果**此后重置帐户锁定计数器** &gt; **ExtranetObservationWindow**，帐户可能被锁定的 AD，但在"延迟的方式"中的可能。 可能需要一段时间才能获取锁定，由 AD 具体取决于你的配置因为 AD FS 将在之前其观察时段内只允许一个不断地尝试密码的帐户**badPwdCount**达到**帐户锁定阈值**.

有关详细信息，请参阅[配置帐户锁定](https://blogs.technet.microsoft.com/secguide/2014/08/13/configuring-account-lockout/)。 

## <a name="known-issues"></a>已知问题
其中的 AD 用户帐户无法进行身份验证使用 AD FS 因为存在是一个已知的问题**badPwdCount**属性不复制到查询 ADFS 的域控制器。 请参阅[2971171](https://support.microsoft.com/help/2971171/adfs-authentication-issue-for-active-directory-users-when-extranet-loc)的更多详细信息。 您可以查找到目前为止已发布的所有 AD FS Qfe[此处](../deployment/updates-for-active-directory-federation-services-ad-fs.md)。

## <a name="key-points-to-remember"></a>要记住的关键点
- Extranet 锁定功能仅适用于**extranet 方案**通过 Web 应用程序代理的身份验证请求的显示位置
- Extranet 锁定功能仅适用于**用户名和密码身份验证**
- AD FS 不会保留任何课程**badPwdCount**或软-锁定的用户。AD FS 使用 AD 跟踪的所有状态
- AD FS 执行用于查找**badPwdCount**通过 LDAP 调用上 PDC 来执行每次身份验证尝试的用户属性  
- 如果它不能访问 PDC，AD FS 早于 2016年将失败。 AD FS 2016 引入了改进，使 AD FS 以故障回复到其他域控制器在 PDC 不可用。 
- AD FS 将允许身份验证请求从 extranet 如果 badPwdCount < ExtranetLockoutThreshold 
- 如果**badPwdCount** >= **ExtranetLockoutThreshold** AND **badPasswordTime** + **ExtranetObservationWindow** < 当前时间，AD FS 将拒绝来自 extranet 的身份验证请求
- 若要避免恶意帐户锁定，您应确保**ExtranetLockoutThreshold** < **帐户锁定阈值**AND **ExtranetObservationWindow**  > **重置帐户锁定计数器**


## <a name="additional-references"></a>其他参考  
- [保护 Active Directory 联合身份验证服务的最佳做法](../../ad-fs/deployment/best-practices-securing-ad-fs.md)
- [向非管理员用户委派 AD FS Powershell Commandlet 访问权限](delegate-ad-fs-pshell-access.md)
- [Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)

    
