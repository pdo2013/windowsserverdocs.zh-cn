---
title: 凭据保护和管理
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 16cc1f2260bf0552da6902dc3e97de65d29c7931
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812198"
---
# <a name="credentials-protection-and-management"></a>凭据保护和管理

>适用于：Windows 服务器 （半年频道），Windows Server 2016

面向 IT 专业人员的本主题讨论了功能和 Windows Server 2012 R2 和 Windows 8.1 进行凭据保护和控制域身份验证以降低凭据被盗中引入的方法。

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>用于远程桌面连接的受限管理模式
受限管理模式提供了一种以交互方式登录到远程主机服务器而无需将凭据传输到该服务器的方法。 这可以防止凭据在初始连接期间被截获（如果服务器已泄露）。

在此模式下使用管理员凭据时，远程桌面客户端将尝试以交互方式登录到主机，该主机同样也支持此模式且无需发送凭据。 如果主机验证连接到它的用户帐户具有管理员权限且支持受限管理模式，则连接已成功。 否则，连接尝试失败。 受限管理模式从不将纯文本或其他可重用形式的凭据发送给远程计算机。

### <a name="lsa-protection"></a>LSA 保护
本地安全机构 (LSA) 包含在本地安全机构安全服务 (LSASS) 进程中，它可以验证用户的本地和远程登录，并且可以强制执行本地安全策略。 Windows 8.1 操作系统提供额外保护为 LSA 以防止非受保护的进程注入代码。 这为 LSA 存储和管理的凭据提供了更高的安全性。 LSA 的此受保护的进程设置可以在 Windows 8.1 中配置，但在 Windows RT 8.1 中默认情况下，无法更改。

有关配置 LSA 保护的信息，请参阅 [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md)。

### <a name="protected-users-security-group"></a>受保护的用户安全组
此新的域全局组将触发非配置设备和主机计算机运行 Windows Server 2012 R2 和 Windows 8.1 上的新保护。 受保护用户组启用了 Windows Server 2012 R2 的域中的域控制器和域的其他保护。 如果用户通过未被破坏的计算机登录到网络上的计算机，这将大大减少可用凭据的类型。

受保护的用户组的成员将受到以下身份限制方法的进一步限制：

-   受保护的用户组成员仅可通过 Kerberos 协议进行登录。 该帐户无法使用 NTLM、摘要式身份验证或 CredSSP 进行身份验证。 在运行 Windows 8.1 的设备，不会缓存密码，因此将使用这些安全支持提供程序 (Ssp) 的任何一个设备将故障域进行身份验证时的帐户是受保护用户组的成员。

-   Kerberos 协议不会在预身份验证过程中使用较弱的 DES 或 RC4 加密类型。 这意味着必须将域配置为至少支持 AES 加密套件。

-   用户的帐户不能被委派了约束或不受约束的 Kerberos 委派。 这意味着，如果用户是受保护的用户组的成员，则之前对其他系统的连接可能会失败。

-   Kerberos 票证授予票证 (TGT) 的生存期设置默认为 4 小时，它可以使用通过 Active Directory 管理中心 (ADAC) 访问的身份验证策略和接收器进行配置。 这意味着超过 4 小时后，用户必须重新进行身份验证。

> [!WARNING]
> 服务和计算机的帐户不应是受保护用户组的成员。 此组不提供本地保护，因为密码或证书在主机上始终可用。 身份验证将失败并出现错误"的用户名或密码不正确"的任何服务或计算机添加到受保护用户组。

有关此组的详细信息，请参阅[受保护的用户安全组](protected-users-security-group.md)。

### <a name="authentication-policy-and-authentication-policy-silos"></a>身份验证策略和身份验证策略接收器
将引入基于林的 Active Directory 策略，并且它们可应用到具有 Windows Server 2012 R2 域功能级别的域中的帐户。 这些身份验证策略可以控制用户用于登录的主机。 它们可以与受保护的用户安全组一起使用，并且管理员可以将身份验证的访问控制条件应用到帐户中。 这些身份验证策略通过隔离相关帐户来限制网络的作用域。

新 Active Directory 对象类，身份验证策略，可将身份验证配置应用到具有 Windows Server 2012 R2 域功能级别的域中的帐户类。 在 Kerberos AS 或 TGS 交换期间，将强制执行身份验证策略。 Active Directory 帐户类包括：

-   “用户”

-   计算机

-   托管服务帐户

-   组托管服务帐户

有关详细信息，请参阅[身份验证策略和身份验证策略接收器](authentication-policies-and-authentication-policy-silos.md)。

有关如何配置受保护的帐户的详细信息，请参阅[如何配置受保护的帐户](how-to-configure-protected-accounts.md)。

## <a name="see-also"></a>请参阅
有关 LSA 和 LSASS 的详细信息，请参阅 [Windows 登录和身份验证的技术概述](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx)。



