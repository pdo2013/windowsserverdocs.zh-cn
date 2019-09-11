---
title: 阻止使用 RC4 机密密钥的 Kerberos 更改密码
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: ec49d195870f87ac8482a7ff6a93a50309425c6c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870377"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>阻止使用 RC4 密钥的 Kerberos 更改密码

>适用于：Windows Server （半年频道）、Windows Server 2016、Windows Server 2008 R2 和 Windows Server 2008

面向 IT 专业人员的本主题介绍 Kerberos 协议中的一些限制，这些限制可能会导致恶意用户控制用户的帐户。 Kerberos 网络身份验证服务（V5）标准（RFC 4120）中存在一个限制，这在行业中是众所周知的，因此攻击者可以以用户身份进行身份验证，也可以更改该用户的密码（如果攻击者知道用户的密钥）。

默认情况下，对用户的密码派生 Kerberos 密钥（RC4 和高级加密标准 [AES]）进行验证时，将在每个 RFC 4757 的 Kerberos 密码更改交换过程中进行验证。 用户的纯文本密码绝不会提供给密钥发行中心（KDC），并且默认情况下，Active Directory 域控制器没有帐户的纯文本密码副本。 如果域控制器不支持 Kerberos 加密类型，则不能使用该密钥来更改密码。 

在本主题开头的 "适用于" 列表中指定的 Windows 操作系统中，有三种方法可以通过使用 Kerberos 和 RC4 密钥来阻止更改密码的功能：

- 将用户帐户配置为包含帐户选项 "交互式登录需要智能卡"。 这会将用户限制为仅使用有效的智能卡登录，以便拒绝 RC4 身份验证服务请求（请求）。 若要设置帐户的帐户选项，请右键单击该帐户，然后单击 "属性"，然后单击 "帐户" 选项卡。 

- 禁用对所有域控制器上的 Kerberos 的 RC4 支持。 这需要至少使用 Windows Server 2008 域功能级别和环境，在此环境中，所有 Kerberos 客户端、应用程序服务器以及与域之间的信任关系都必须支持 AES。 Windows Server 2008 和 Windows Vista 中引入了对 AES 的支持。

    [!NOTE]
    禁用 RC4 会出现一个已知问题，这可能会导致系统重启。 请参阅以下修补程序：
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - 对于早期版本的 Windows Server，无修补程序可用

- 部署设置为 Windows Server 2012 R2 域功能级别或更高版本的域，并将用户配置为受保护的用户安全组的成员。 由于此功能只会干扰 Kerberos 协议中的 RC4 使用，因此请参阅下面的 "[另请参见](#see-also)" 部分中的资源。

## <a name="see-also"></a>请参阅

- 有关如何防止在 Windows Server 2012 R2 域中使用 RC4 加密类型的信息，请参阅[受保护的用户安全组](/../credentials-protection-and-management/protected-users-security-group.md)和[如何配置受保护的帐户](/../credentials-protection-and-management/how-to-configure-protected-accounts.md)。

- 有关 RFC 4120 和 RFC 4757 的说明，请参阅[IETF 文档](http://tools.ietf.org/html/)。
