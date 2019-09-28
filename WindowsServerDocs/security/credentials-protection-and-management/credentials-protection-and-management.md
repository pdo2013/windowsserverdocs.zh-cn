---
title: 凭据保护和管理
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 31f0f89099a71f8ea6abcf0064113d6af9608c5a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403798"
---
# <a name="credentials-protection-and-management"></a>凭据保护和管理

>适用于：Windows Server（半年频道）、Windows Server 2016

适用于 IT 专业人员的本主题讨论了在 Windows Server 2012 R2 中引入的功能和方法，并 Windows 8.1 凭据保护和域身份验证控件来减少凭据被盗。

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>用于远程桌面连接的受限管理模式
受限管理模式提供了一种以交互方式登录到远程主机服务器而无需将凭据传输到该服务器的方法。 这可以防止凭据在初始连接期间被截获（如果服务器已泄露）。

在此模式下使用管理员凭据时，远程桌面客户端将尝试以交互方式登录到主机，该主机同样也支持此模式且无需发送凭据。 如果主机验证连接到它的用户帐户具有管理员权限且支持受限管理模式，则连接已成功。 否则，连接尝试失败。 受限管理模式从不将纯文本或其他可重用形式的凭据发送给远程计算机。

### <a name="lsa-protection"></a>LSA 保护
本地安全机构 (LSA) 包含在本地安全机构安全服务 (LSASS) 进程中，它可以验证用户的本地和远程登录，并且可以强制执行本地安全策略。 Windows 8.1 操作系统为 LSA 提供附加保护，以防止未受保护的进程注入代码。 这为 LSA 存储和管理的凭据提供了更高的安全性。 可以在 Windows 8.1 中配置 LSA 的这一受保护的过程设置，但默认情况下在 Windows RT 8.1 中是启用的，不能更改。

有关配置 LSA 保护的信息，请参阅 [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md)。

### <a name="protected-users-security-group"></a>受保护的用户安全组
这一新的域全局组在运行 Windows Server 2012 R2 和 Windows 8.1 的设备和主机计算机上触发了新的不可配置的保护。 受保护的用户组为 Windows Server 2012 R2 域中的域控制器和域启用了其他保护。 如果用户通过未被破坏的计算机登录到网络上的计算机，这将大大减少可用凭据的类型。

受保护的用户组的成员将受到以下身份限制方法的进一步限制：

-   受保护的用户组成员仅可通过 Kerberos 协议进行登录。 该帐户无法使用 NTLM、摘要式身份验证或 CredSSP 进行身份验证。 在运行 Windows 8.1 的设备上，不会缓存密码，因此当该帐户是受保护用户组的成员时，使用这些安全支持提供程序（Ssp）之一的设备将无法对域进行身份验证。

-   Kerberos 协议不会在预身份验证过程中使用较弱的 DES 或 RC4 加密类型。 这意味着必须将域配置为至少支持 AES 加密套件。

-   用户的帐户不能用 Kerberos 约束或非约束委托委托。 这意味着，如果用户是受保护的用户组的成员，则之前对其他系统的连接可能会失败。

-   Kerberos 票证授予票证 (TGT) 的生存期设置默认为 4 小时，它可以使用通过 Active Directory 管理中心 (ADAC) 访问的身份验证策略和接收器进行配置。 这意味着超过 4 小时后，用户必须重新进行身份验证。

> [!WARNING]
> 服务和计算机的帐户不应是受保护用户组的成员。 此组不提供本地保护，因为密码或证书在主机上始终可用。 对于添加到受保护用户组的任何服务或计算机，身份验证将失败，并出现错误 "用户名或密码不正确"。

有关此组的详细信息，请参阅[受保护的用户安全组](protected-users-security-group.md)。

### <a name="authentication-policy-and-authentication-policy-silos"></a>身份验证策略和身份验证策略接收器
引入基于林的 Active Directory 策略，这些策略可应用于具有 Windows Server 2012 R2 域功能级别的域中的帐户。 这些身份验证策略可以控制用户用于登录的主机。 它们可以与受保护的用户安全组一起使用，并且管理员可以将身份验证的访问控制条件应用到帐户中。 这些身份验证策略通过隔离相关帐户来限制网络的作用域。

新的 Active Directory 对象类 "身份验证策略" 允许将身份验证配置应用到具有 Windows Server 2012 R2 域功能级别的域中的帐户类。 在 Kerberos AS 或 TGS 交换期间，将强制执行身份验证策略。 Active Directory 帐户类包括：

-   “用户”

-   计算机

-   托管服务帐户

-   组托管服务帐户

有关详细信息，请参阅[身份验证策略和身份验证策略接收器](authentication-policies-and-authentication-policy-silos.md)。

有关如何配置受保护的帐户的详细信息，请参阅[如何配置受保护的帐户](how-to-configure-protected-accounts.md)。

## <a name="see-also"></a>请参阅
有关 LSA 和 LSASS 的详细信息，请参阅 [Windows 登录和身份验证的技术概述](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx)。



