---
title: 阻止 Kerberos 更改使用 RC4 机密密钥的密码
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816268"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>阻止使用 RC4 密钥的 Kerberos 更改密码

>适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2008 R2 和 Windows Server 2008

面向 IT 专业人员的本主题介绍中的用户帐户控制的恶意用户可能会导致 Kerberos 协议的一些限制。 没有在 Kerberos 网络身份验证服务 (V5) 标准 (RFC 4120) 行业，凭此攻击者可以以用户身份进行身份验证或更改该用户的密码，如果攻击者知道用户的机密密钥中的已知限制。

拥有用户的密码派生 Kerberos 机密密钥 （RC4 和高级加密标准 [AES] 默认情况下） 在每个 RFC 4757 Kerberos 密码更改交换过程进行验证。 用户的纯文本密码永远不会提供到密钥分发中心 (KDC) 中，并且默认情况下，Active Directory 域控制器并不具有帐户的纯文本密码的副本。 如果域控制器不支持 Kerberos 加密类型，而密钥不能用于更改密码。 

在本主题开头适用于列表中指定的 Windows 操作系统，有三种方法可以阻止使用 RC4 密钥使用 Kerberos 来更改密码的功能：

- 配置用户帐户，以包含智能卡的帐户选项是所必需的交互式登录。 这将限制用户，以便将拒绝 RC4 的身份验证服务请求 （AS 请求） 仅使用有效的智能卡登录。 若要设置的帐户上的帐户选项，在帐户中，单击属性中，右键单击，然后单击帐户选项卡。 

- 禁用 RC4 支持 Kerberos 的所有域控制器上。 这要求至少需要 Windows Server 2008 域功能级别和所有 Kerberos 客户端、 应用程序服务器和域中的信任关系必须都支持 AES 的环境。 Windows Server 2008 和 Windows Vista 中引入了对 AES 的支持。

    [!NOTE]
    没有禁用 RC4 这可能导致系统重新启动的已知的问题。 请参阅以下修补程序：
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - 无修补程序是适用于 Windows Server 的早期版本

- 部署域组，为 Windows Server 2012 R2 域功能级别或更高版本，并将用户配置为受保护用户安全组的成员。 因为此功能会中断 Kerberos 协议中的不止是 RC4 使用情况，请参阅以下中的资源[另请参阅](#see-also)部分。

## <a name="see-also"></a>请参阅

- 有关如何避免 Windows Server 2012 R2 的域中的 RC4 加密类型的使用情况的信息，请参阅[Protected Users Security Group](/../credentials-protection-and-management/protected-users-security-group.md)，并[How to Configure Protected Accounts](/../credentials-protection-and-management/how-to-configure-protected-accounts.md)。

- 有关 RFC 4120 和 RFC 4757 的说明，请参阅[IETF 文档](http://tools.ietf.org/html/)。
