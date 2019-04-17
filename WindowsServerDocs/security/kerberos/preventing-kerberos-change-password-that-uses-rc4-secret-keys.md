---
title: "阻止使用 RC4 机密键的 Kerberos 更改密码"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>阻止使用 RC4 密钥 Kerberos 更改密码

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2008R2 和 Windows Server 2008

此主题以供 IT 专业人员解释可能导致恶意用户的用户帐户控制 Kerberos 协议中的一些限制。 没有中的限制 Kerberos 网络身份验证服务 (V5) 标准 (RFC 4120)，这是已知的由此攻击可以的用户身份验证或更改该用户的密码，如果攻击了解用户的密钥行业内。

拥有一个用户密码派生 Kerberos 密钥（RC4 和高级加密标准 [AES] 默认）验证在每个 RFC 4757 交换 Kerberos 密码更改。 用户纯文本密码永远不会提供给密钥 Distribution 中心 (KDC)，并且默认情况下，Active Directory 域控制器不具备一份纯文本的帐户的密码。 如果域控制器 Kerberos 加密类型不支持，该密钥不能用于更改密码。 

在适用于列表开头的本主题中指定 Windows 操作系统，有以下三种方法可以阻止能够通过使用 RC4 密钥 Kerberos 更改密码：

- 配置用户帐户，以包含智能卡的帐户选项，才能进行交互登录。 这将限制为仅以便 RC4（AS 要求）身份验证服务请求被拒绝使用有效的智能卡登录的用户。 若要设置帐户上的帐户选项，在帐户上，单击属性，请右键单击，然后单击帐户选项卡。 

- 禁用所有域控制器上的 Kerberos RC4 支持。 Windows Server 2008 域功能级别和所有 Kerberos 客户端应用程序服务器，信任关系来回域必须都支持 AES 环境至少需要此。 在 Windows Server 2008 和 Windows Vista 引入了 AES 的支持。

    [!NOTE]
    可通过禁用显示器的 RC4 这可能会导致系统重新启动的已知的问题。 请参阅下面的修补程序：
    - [Windows Server 2012R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - 未的修复程序是适用于以前版本的 Windows Server

- 部署域设置为 Windows Server 2012 R2 域功能级别或更高版本，并且配置为保护用户安全组成员的用户。 因为此功能会打乱不仅仅 RC4 Kerberos 协议中的使用量，请参阅以下资源[另请参阅](#see-also)部分。

## <a name="see-also"></a>请参阅

- 有关如何阻止 Windows Server 2012 R2 域中 RC4 加密类型的使用率信息，请参阅[保护用户安全组](/../credentials-protection-and-management/protected-users-security-group.md)，并[如何配置保护帐户](/../credentials-protection-and-management/how-to-configure-protected-accounts.md)。

- 有关 RFC 4120 和 RFC 4757 说明，请参阅[IETF 文档](http://tools.ietf.org/html/)。
