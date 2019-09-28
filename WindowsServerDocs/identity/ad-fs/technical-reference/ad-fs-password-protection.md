---
title: AD FS 密码攻击保护
description: 本文档介绍如何保护 AD FS 用户免受密码攻击
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 68624e2307e5ddefc6e32160cabfcd140f609966
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407240"
---
# <a name="ad-fs-password-attack-protection"></a>AD FS 密码攻击保护

## <a name="what-is-a-password-attack"></a>什么是密码攻击？

联合单一登录的要求是终结点的可用性，通过 internet 进行身份验证。 Internet 上的身份验证终结点的可用性允许用户访问应用程序，即使它们不在公司网络中也是如此。 

但是，这也意味着某些不良的执行组件可以利用 internet 上提供的联合终结点，并使用这些终结点来尝试和确定密码或创建拒绝服务攻击。 一种变得更常见的攻击称为**密码攻击**。 

共有两种类型的常见密码攻击。 密码喷涂攻击 & 暴力破解密码攻击。 

### <a name="password-spray-attack"></a>密码喷涂攻击
在密码喷涂攻击中，这些不良的执行组件将在多个不同的帐户和服务中尝试使用最常见的密码，以访问他们可以找到的任何受密码保护的资产。 通常，它们涵盖许多不同的组织和标识提供者。 例如，攻击者将使用一个通常可用的工具包来枚举多个组织中的所有用户，然后针对所有这些帐户尝试 "P @ $ $w 0rd" 和 "Password1"。 为了给出这种想法，攻击可能如下所示：


|  目标用户   | 目标密码 |
|----------------|-----------------|
| User1@org1.com |    Password1    |
| User2@org1.com |    Password1    |
| User1@org2.com |    Password1    |
| User2@org2.com |    Password1    |
|       …        |        …        |
| User1@org1.com |    P @ $ $w 0rd     |
| User2@org1.com |    P @ $ $w 0rd     |
| User1@org2.com |    P @ $ $w 0rd     |
| User2@org2.com |    P @ $ $w 0rd     |

这种攻击模式 evades 大多数检测技术，因为在单个用户或公司的 vantage 点，攻击就好像是隔离的失败登录。

对于攻击者而言，这是一场游戏：他们知道有一些密码非常常见。  攻击者对每一千个帐户进行了一些成功的攻击，这就足够了。 他们使用这些帐户从电子邮件获取数据、获取联系人信息并发送仿冒网站链接，或只展开密码喷涂目标组。 攻击者并不关心那些初始目标的工作，只是他们能够利用一些成功的内容。

但通过执行几个步骤来正确配置 AD FS 和网络，可以根据这些类型的攻击来保护 AD FS 终结点。 本文介绍3个需要正确配置的区域，以帮助防范这些攻击。

### <a name="brute-force-password-attack"></a>强力密码攻击 
在这种形式的攻击中，攻击者将尝试对目标帐户集进行多次密码尝试。 在许多情况下，这些帐户将针对在组织内具有更高级别访问权限的用户。 它们可能是组织内的管理人员或管理关键基础结构的管理员。  

这种类型的攻击也可能导致 DOS 模式。 这可能是服务级别，ADFS 无法处理大量请求，因为服务器数不足，或者可能是用户在锁定其帐户的用户级别。  

## <a name="securing-ad-fs-against-password-attacks"></a>保护 AD FS 免受密码攻击 

但通过执行几个步骤来正确配置 AD FS 和网络，可以根据这些类型的攻击来保护 AD FS 终结点。 本文介绍3个需要正确配置的区域，以帮助防范这些攻击。 


- 级别1，基线：这些是必须在 AD FS 服务器上配置的基本设置，以确保不良执行组件不能暴力破解攻击联合用户。 
- 级别2：保护 extranet：这些是必须配置的设置，以确保将 extranet 访问权限配置为使用安全协议、身份验证策略和合适的应用程序。 
- 第3级：移动到无密码，以进行 extranet 访问：这些是高级设置和指导原则，可让你使用更安全的凭据（而不是容易受到攻击的密码）访问联合资源。 

## <a name="level-1-baseline"></a>级别1：Baseline

1. 如果 ADFS 2016，实现[extranet 智能锁定](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)extranet 智能锁定会跟踪熟悉的位置，并允许有效用户在以前从该位置成功登录。 通过使用 extranet 智能锁定，你可以确保不良的执行组件将无法对用户进行暴力攻击，同时会使合法用户能够高效工作。
    - 如果你不在 AD FS 2016 上，我们强烈建议你[升级](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md)到 AD FS 2016。 它是 AD FS 2012 R2 的简单升级路径。 如果你在 AD FS 2012 R2 上，则实现[extranet 锁定](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)。 此方法的一个缺点是，如果您采用暴力破解模式，可能会阻止有效用户进行 extranet 访问。 服务器2016上的 AD FS 没有此缺点。

2. 监视 & 阻止可疑 IP 地址 
    - 如果有 Azure AD Premium，请为 ADFS 实现连接运行状况，并使用其提供的有[风险的 IP 报告](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview)通知。

        a. 授权并非适用于所有用户，并且需要25个许可证/ADFS/WAP 服务器，这对于客户来说可能很简单。

        b. 你现在可以调查正在生成大量失败登录的 IP

        c. 这将要求你在 ADFS 服务器上启用审核。

3.  阻止可疑 IP。  这可能会阻止 DOS 攻击。

    a. 如果在2016，请使用[*Extranet 禁止的 IP 地址*](../../ad-fs/operations/configure-ad-fs-banned-ip.md)功能来阻止 #3 （或手动分析）标记的来自 IP 的任何请求。

    b. 如果你在 AD FS 2012 R2 或更低版本上，请在 Exchange Online 中直接阻止 IP 地址，并根据需要在防火墙上进行选择。

4. 如果有 Azure AD Premium，请使用[Azure AD 密码保护](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises)来防止猜出密码进入 Azure AD  

    a. 请注意，如果有猜出的密码，只需1-3 次尝试即可破解密码。 此功能可防止设置这些设置。 

    b. 根据预览统计信息，几乎 20-50% 的新密码被阻止设置。 这意味着，% 的用户很容易破解密码。

## <a name="level-2-protect-your-extranet"></a>级别2：保护 extranet

5. 对于从 extranet 访问的任何客户端，移动到新式身份验证。 邮件客户端是此的主要部分。 

    a. 你将需要使用适用于移动设备的 Outlook Mobile。 新的 iOS 本机邮件应用还支持新式身份验证。 

    b. 你将需要使用 Outlook 2013 （带有最新的 CU 修补程序）或 Outlook 2016。

6. 针对所有 extranet 访问启用 MFA。 这为你提供了对任何 extranet 访问的保护。

   a.  如果你有 Azure AD 高级版，请使用[Azure AD 条件性访问策略](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)来控制这种情况。  这比在 AD FS 实现规则要好。  这是因为现代的客户端应用程序会更频繁地强制执行。  这会在 Azure AD 时使用刷新令牌请求新的访问令牌（通常每小时一次）。  

   b.  如果你没有 Azure AD 高级版或在 AD FS 上提供了允许基于 internet 的访问的其他应用，则实现 MFA （也可以是 Azure MFA，也可以是 AD FS 2016），并为所有 extranet 访问执行[全局 MFA 策略](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally)。

## <a name="level-3-move-to-password-less-for-extranet-access"></a>级别3：转到不使用 extranet 访问的密码

7. 转到 Windows 10 并使用[Hello 企业版](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)。

8. 对于其他设备，如果在 AD FS 2016 上，你可以将[AZURE MFA OTP](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md)用作第一个因素，将密码用作第二个因素。 

9. 对于移动设备，如果只允许 MDM 托管设备，则可以使用[证书](../../ad-fs/operations/configure-user-certificate-authentication.md)在中记录用户。 

## <a name="urgent-handling"></a>紧急处理

如果 AD FS 环境处于活动状态，则应尽早实现以下步骤：

 - 在 ADFS 中禁用 U/P 终结点，并要求每个人都可以访问或进入你的网络。 这要求你完成步骤**级别 2 #1a** 。 否则，所有内部 Outlook 请求仍将通过 EXO 除外代理身份验证通过云进行路由。
 - 如果攻击只是通过 EXO 除外进行的，则可以使用身份验证策略对 Exchange 协议（POP、IMAP、SMTP、EWS 等）禁用基本身份验证，这些协议和身份验证方法将在大多数这些攻击中使用。 此外，EXO 除外和每邮箱协议支持中的客户端访问规则会在身份验证后进行评估，并且不会帮助减少攻击。 
 - 使用第3级（#1 3）有选择地提供 extranet 访问。

## <a name="next-steps"></a>后续步骤

- [升级到 AD FS server 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [AD FS 2016 中的 Extranet 智能锁定](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [配置条件访问策略](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Azure AD 密码保护](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
