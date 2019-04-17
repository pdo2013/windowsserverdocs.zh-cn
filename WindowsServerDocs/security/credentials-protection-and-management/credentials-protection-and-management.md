---
title: "凭据保护和管理"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-protection-and-management"></a>凭据保护和管理

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员讨论功能和 Windows Server 2012 R2 和 Windows 8.1 的凭据保护和域身份验证的控件，以减少凭据被盗中引入的方法。

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>远程桌面连接受限的管理员模式
限制的管理员模式提供了不传输到服务器凭据交互登录到主机远程服务器的方法。 这可以防止你的凭据正在获取初始连接的过程中，如果该服务器已受到威胁。

远程桌面客户端以管理员身份使用此模式下，尝试交互登录到主机还支持无需发送凭据的此模式。 当连接到它的用户帐户使用具有管理员权限，并支持受限管理员模式，用于验证主机时，在连接成功。 否则，连接尝试无法正常工作。 在任意点发送纯文本或到远程计算机的凭据其他重用表单不如此限制的管理员模式。

### <a name="lsa-protection"></a>LSA 保护
本地安全颁发机构 (LSA)，在本地安全颁发机构安全服务 (LSASS) 进程所在，验证本地边远登录的用户，并执行本地安全策略。 Windows 8.1 操作系统提供了更多保护 LSA 由不受保护的进程阻止代码。 这提供 LSA 存储，并管理的凭据增加的安全性。 LSA 此受保护的进程设置可以配置 Windows 8.1 中，但 Windows RT 8.1 中的默认情况下处于打开状态，无法更改。

有关配置 LSA 保护信息，请参阅[配置其他 LSA 防护](configuring-additional-lsa-protection.md)。

### <a name="protected-users-security-group"></a>受保护的用户安全组
此新域全球组触发设备和主计算机运行的 Windows Server 2012 R2 和 Windows 8.1 的新的可配置保护。 保护用户组可使其他受到保护域控制器和 Windows Server 2012 R2 域中的域。 此大大用户登录到该网络上计算机从非威胁计算机时减少了可用的凭据类型。

保护用户组成员受到进一步由以下的身份验证方法：

-   组成员的受保护的用户可以仅使用 Kerberos 协议进行登录。 无法验证帐户，使用 NTLM、简要身份验证或 CredSSP。 在设备上运行的 Windows 8.1，密码不会缓存，以便将无法验证到某个域，保护用户组成员的帐户时的设备，将使用这些安全支持提供商 (Ssp) 的任何一个。

-   Kerberos 协议将不预身份验证过程中使用较弱 DES 或 RC4 加密类型。 这意味着必须配置域支持至少 AES 加密套件。

-   用户帐户不能委派与 Kerberos 受限或不受约束委派。 这意味着前者连接到其他系统可能会失败，用户是否受到保护用户组成员。

-   默认 Kerberos 票证授予门票 (Tgt) 整个使用期限内是可使用身份验证的策略和访问过的 Active Directory 管理中心 (ADAC) 通过思配置设置的 4 个小时。 这意味着，过后四个小时，用户必须进行身份验证再次。

> [!WARNING]
> 服务和计算机的帐户不应保护用户组中的成员。 此组提供没有本地保护由于密码或证书始终在主机上可用。 身份验证将失败并显示错误"的用户名和密码不正确"的任何服务或添加到保护用户组中的计算机。

有关此组详细信息，请参阅[保护用户安全组](protected-users-security-group.md)。

### <a name="authentication-policy-and-authentication-policy-silos"></a>身份验证的策略和身份验证策略思
引入了基于森林 Active Directory 策略，并可用于与 Windows Server 2012 R2 域功能级别域中的帐户。 这些身份验证策略可以控制哪些主机用户可以使用它来登录。 他们配合使用保护用户安全组中，并管理员可以适用于帐户的身份验证访问控制条件。 这些身份验证策略隔离相关的帐户可将限制网络的范围。

新的 Active Directory 对象类别，身份验证策略允许你为与 Windows Server 2012 R2 域功能级别域中的帐户类应用身份验证配置。 身份验证策略强制执行期间 Kerberos AS 或 TGS 更换费用。 Active Directory 帐户类是：

-   用户

-   计算机

-   托管服务帐户

-   组托管的服务帐户

有关详细信息，请参阅[身份验证的策略和身份验证策略思](authentication-policies-and-authentication-policy-silos.md)。

有关详细信息如何配置受保护的帐户，请参阅[如何配置保护帐户](how-to-configure-protected-accounts.md)。

## <a name="see-also"></a>请参阅
有关 LSA 和 LSASS 的详细信息，请参阅[Windows 登录并验证 Technical 概述](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx)。



