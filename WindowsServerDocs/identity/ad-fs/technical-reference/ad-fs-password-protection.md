---
title: AD FS 密码攻击保护
description: 本文档介绍如何从密码攻击保护 AD FS 用户
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de1af9712b54c977c591953c68eec506c80d3cdd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821998"
---
# <a name="ad-fs-password-attack-protection"></a>AD FS 密码攻击保护

## <a name="what-is-a-password-attack"></a>什么是密码攻击？

联合单一登录的要求是用于通过 internet 进行身份验证的终结点的可用性。 在 internet 上的身份验证终结点的可用性，用户可以访问的应用程序，即使它们不在公司网络上。 

但是，这也意味着一些不良参与方，可以在 internet 上充分利用可用的联合端点并使用这些终结点，并尝试确定密码或创建的拒绝服务攻击。 调用一个此类攻击正变得更常见**密码攻击**。 

有两种类型的常见密码攻击。 密码喷射攻击和暴力攻击强制密码攻击。 

### <a name="password-spray-attack"></a>密码喷射攻击
在密码喷射攻击中，这些不良参与方将跨许多不同的帐户和服务以获取对用户可从中找到任何受密码保护的资产的访问尝试的最常见的密码。 通常这些跨许多不同的组织和标识提供程序。 例如，攻击者将使用一个常用工具包枚举所有在组织中多个用户，然后再尝试"P@$$w0rd"和"Password1"对所有这些帐户。 为了让你了解，攻击可能如下所示：

|目标用户|目标密码|
|-----|-----|-----|
|User1@org1.com|Password1|
|User2@org1.com|Password1|
|User1@org2.com|Password1|
|User2@org2.com|Password1|
|…|…|
|User1@org1.com|P@$$w0rd|
|User2@org1.com|P@$$w0rd|
|User1@org2.com|P@$$w0rd|
|User2@org2.com|P@$$w0rd|

此攻击模式漏过大多数的检测技术，因为从单个用户或公司的出发点，攻击看起来就像独立的失败登录。

攻击者，这是一个数字游戏： 他们知道有一些密码有非常常见的。  攻击者将收到一些成功的攻击，每个几千个帐户和这足以生效。 它们使用的帐户来从电子邮件中获取数据、 获取联系信息，然后发送仿冒网站的链接，或只需展开密码喷射目标组。 攻击者不太在意这些初始目标是谁，只是它们具有他们可以利用某些成功。

它们使用的帐户来从电子邮件中获取数据、 获取联系信息，然后发送仿冒网站的链接，或只需展开密码喷射目标组。 攻击者不太在意这些初始目标是谁，只是它们具有他们可以利用某些成功。

但可以通过执行一些步骤来配置 AD FS 和正确的网络，针对这些类型的攻击中保护 AD FS 终结点。 本文介绍了 3 个领域的需要正确配置以帮助防范这些攻击。

### <a name="brute-force-password-attack"></a>暴力破解密码攻击 
在这种形式的攻击，攻击者将尝试对一组目标帐户的多个密码尝试。 在许多情况下这些帐户将针对具有更高级别的组织内的访问权限的用户。 这些可能是高级管理人员在机构或管理员管理关键基础设施。  

此类攻击还可能导致 DOS 的模式。 这可能是在 ADFS 是无法处理大型 # 由于没有足够数量的服务器请求的服务级别，或可能是在用户无法使用其帐户被锁定在其中用户级别。  

## <a name="securing-ad-fs-against-password-attacks"></a>保护 AD FS 密码攻击 
 
但可以通过执行一些步骤来配置 AD FS 和正确的网络，针对这些类型的攻击中保护 AD FS 终结点。 本文介绍了 3 个领域的需要正确配置以帮助防范这些攻击。 


- 第 1 级，基线：这些是必须要确保不良参与方无法暴力破解攻击联合用户的 AD FS 服务器配置基本设置。 
- 级别 2，保护 extranet:这些是必须配置为确保 extranet 访问配置为使用安全的协议、 身份验证策略和相应的应用程序的设置。 
- 级别 3，转到无密码进行 extranet 访问：这些都被高级设置和指导原则，以实现更安全的凭据而不是个密码，这是容易受到攻击的联合资源访问权限。 

## <a name="level-1-baseline"></a>级别 1:Baseline

1. 如果 ADFS 2016，实现[extranet 的智能锁定](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)Extranet 的智能锁定跟踪熟悉的位置，并将允许它们之前已登录成功从该位置如果涉及到一个有效用户。 通过使用 extranet 的智能锁定，可以确保不良参与方将无法再到暴力破解攻击用户并同时允许合法用户提高工作效率。
    - 如果你不在 AD FS 2016 中，我们强烈建议您[升级](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md)到 AD FS 2016。 它是从 AD FS 2012 R2 升级路径简单。 如果 AD FS 2012 R2 上，实现[extranet 锁定](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)。 这种方法的一个缺点是有效用户可能会阻止来自 extranet 的访问中，如果你在暴力破解模式。 Server 2016 上的 AD FS 没有这个缺点。

2. 监视和阻止可疑的 IP 地址 
    - 如果你有 Azure AD Premium，ADFS 和使用实现连接运行状况[风险 IP 报表](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview)通知它所提供的。
        
        a. 许可的所有用户并不需要 25 许可证/ADFS/WAP 服务器可能是简单的客户。
    
        b. 现在，可以调查 IP 的生成大型失败的登录数
    
        c. 这将需要在 ADFS 服务器上启用审核。

3.  阻止可疑 IP。  这可能会阻止 DOS 攻击。

    a. 如果在 2016 中，使用[ *Extranet 阻止 IP 地址*](../../ad-fs/operations/configure-ad-fs-banned-ip.md)标记 #3 （或手动分析） 的功能来阻止从 IP 的任何请求。

    b. 如果要在 AD FS 2012 R2 或更低，阻止直接在 Exchange Online 和 （可选） 在防火墙上的 IP 地址。

4. 如果你有 Azure AD Premium，使用[Azure AD 密码保护](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises)，以防被猜出密码导入到 Azure AD  

    a. 请注意，是否您有被猜出密码，你可以用它们只需 1-3 尝试。 此功能可防止这些设置。 

    b. 从我们预览统计信息中，几乎 20 到 50%的新密码获取阻止设置。 这意味着在 %的用户都可能很容易猜出密码。

## <a name="level-2-protect-your-extranet"></a>级别 2:保护 extranet

5. 将移动到新式身份验证的任何客户端从 extranet 访问。 邮件客户端程序的重要部分。 

    a. 您需要使用 Outlook Mobile 为移动设备。 新的 iOS 本机邮件应用程序支持以及现代身份验证。 

    b. 你将需要使用 Outlook 2013 （采用最新 CU 修补程序） 或 Outlook 2016。

6.  针对所有 extranet 访问启用 MFA。 这样，添加了任何外部网络访问保护。

    a.  如果你有 Azure AD 高级版，使用[Azure AD 条件性访问策略](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)若要对此进行控制。  这是优于实现在 AD FS 规则。  这是因为现代客户端应用程序会更频繁地强制执行。  发生这种情况，在 Azure AD 中，请求新访问令牌 （通常每隔一小时） 使用刷新令牌时。  

    b.  如果你没有 Azure AD 高级版或具有 AD FS 允许 internet 上的其他应用基于访问权限，实现的 MFA （可以是 Azure MFA 还在 AD FS 2016） 和执行[全局 MFA 策略](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally)针对所有 extranet 访问。
 
## <a name="level-3-move-to-password-less-for-extranet-access"></a>级别 3:转到密码进行 extranet 访问更少

7. 将移动到 Window 10 并用[Hello For Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)。

8. 对于其他设备，如果在 AD FS 2016，可以使用[Azure MFA OTP](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md)作为第一个身份和密码作为第二个因素。 

9.  对于移动设备，如果只允许 MDM 托管设备，则可以使用[证书](../../ad-fs/operations/configure-user-certificate-authentication.md)登录用户。 
 
## <a name="urgent-handling"></a>紧急处理

如果 AD FS 环境受到 active 攻击，应尽早实现以下步骤：

 - 禁用 U/P ADFS 中的终结点，然后要求每个人对 VPN 以获取访问权限，或位于内部网络。 这需要你具有步骤**级别 2 #1a**已完成。 否则，所有内部 Outlook 将仍将请求路由通过在云中通过 EXO 代理身份验证。
 - 如果通过 EXO 仅传入攻击，则可以禁用交换协议 （POP、 IMAP、 SMTP、 EWS 等） 的基本身份验证使用身份验证策略，这些协议和身份验证方法正在使用的大多数这些攻击。 此外中 EXO 和每个邮箱协议支持, 的客户端访问规则是计算后的身份验证，并且不会帮助减轻攻击。 
 - 有选择地提供了使用级别 3 #1-3 的 extranet 访问。

## <a name="next-steps"></a>后续步骤

- [升级到 AD FS 服务器 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Extranet AD FS 2016 中的智能锁定](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [配置条件性访问策略](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Azure AD 密码保护](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
