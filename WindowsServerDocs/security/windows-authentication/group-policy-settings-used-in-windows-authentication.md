---
title: Windows 身份验证中使用的组策略设置
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 38df2033b57c0394b96f539f54efe6a3579500f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847928"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Windows 身份验证中使用的组策略设置

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本参考主题面向 IT 专业人员介绍的使用和身份验证过程中的组策略设置的影响。

通过将用户、 计算机和服务帐户添加到组，然后通过将身份验证策略应用于这些组，你可以管理 Windows 操作系统中的身份验证。 这些策略被定义为本地安全策略和管理模板，也称为组策略设置。 可以配置这两个集，并将其分布在整个组织使用组策略。

> [!NOTE]
> Windows Server 2012 R2 中引入的功能让你配置目标服务的身份验证策略或应用程序，通常称为身份验证接收器，通过使用受保护的帐户。 有关如何执行此操作在 Active Directory 中的信息，请参阅[How to Configure Protected Accounts](how-to-configure-protected-accounts.md)。

例如，可以将以下策略应用到基于在组织中的功能的组：

-   本地或域登录

-   通过网络登录

-   重置帐户

-   创建帐户

下表列出了与身份验证相关的策略组，并提供指向文档可帮助你配置这些策略。

|策略组|Location|描述|
|--------|------|--------|
|**密码策略**|本地计算机策略 \ 计算机配置 \windows 设置 \ 安全设置 Settings\Account 策略|密码策略影响的特征和行为的密码。 密码策略用于域帐户或本地用户帐户。 它们确定对于密码，如强制和生存期的设置。<br /><br />有关特定设置的信息，请参阅[密码策略](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy)。|
|**帐户锁定策略**|本地计算机策略 \ 计算机配置 \windows 设置 \ 安全设置 Settings\Account 策略|帐户锁定策略选项集数量的失败的登录尝试后禁用帐户。 使用这些选项可以帮助您检测和阻止的密码被破坏。<br /><br />有关帐户锁定策略选项的信息，请参阅[帐户锁定策略](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy)。|
|**Kerberos 策略**|本地计算机策略 \ 计算机配置 \windows 设置 \ 安全设置 Settings\Account 策略|与 Kerberos 相关的设置包括票证生存期和强制执行规则。 因为 Kerberos 身份验证协议不使用本地帐户进行身份验证 Kerberos 策略不适用于本地帐户数据库。 因此，Kerberos 策略设置可以配置只能通过默认域组策略对象 (GPO)，其中，它会影响域登录。<br /><br />有关域控制器的 Kerberos 策略选项的信息，请参阅[Kerberos 策略](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy)。|
|**审核策略**|Local Computer Policy\Computer Configuration\Windows Settings\Security Settings\Local Policies\Audit Policy|审核策略可以控制和理解对对象，如文件和文件夹，以及管理用户和组帐户和用户登录和注销来的访问。 审核策略可以指定你想要审核，设置大小和行为的安全日志，并确定你想要监视的访问的哪些对象以及想要监视的访问类型的事件的类别。<br /><br />|
|**用户权限分配**|本地计算机策略 \ 计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 策略 \ 用户权限分配|通常根据用户所属，如管理员、 高级用户或用户的安全组分配用户权限。 此类别中的策略设置通常用于授予或拒绝权限来访问基于访问权限和安全性的组成员身份的方法的计算机。|
|**安全选项**|本地计算机策略 \ 计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 安全选项|与身份验证相关的策略包括：<br /><br />-设备<br />域控制器<br />域成员<br />交互式登录<br />Microsoft 网络服务器<br />-网络访问权限<br />-网络安全<br />恢复控制台<br />关闭<br /><br />|
|**凭据委派**|Computer Configuration\Administrative Templates\System\Credentials Delegation|凭据委派是一种机制，允许在其他系统，最值得注意的是成员服务器和域中的域控制器上使用的本地凭据。 这些设置适用于应用程序使用凭据安全支持提供程序 (Cred SSP)。 远程桌面连接是一个示例。|
|**KDC**|Computer Configuration\Administrative Templates\System\KDC|这些策略设置会影响是在域控制器上的服务，密钥分发中心 (KDC) 如何处理 Kerberos 身份验证请求。|
|**Kerberos**|Computer Configuration\Administrative Templates\System\Kerberos|这些策略设置会影响如何配置 Kerberos 以处理支持声明、 Kerberos 保护、 复合身份验证、 标识代理服务器，以及其他配置。|
|**登录**|计算机配置\管理模板\系统\登录|这些策略设置控制系统是如何显示用户的登录体验。|
|**Net Logon**|Computer Configuration\Administrative Templates\System\Net Logon|这些策略设置控制如何在系统处理网络登录请求包括域控制器定位程序的行为方式。<br /><br />有关如何将域控制器定位程序适用于复制进程详细信息，请参阅[了解站点间复制](https://technet.microsoft.com/library/cc771251.aspx)。|
|**生物识别**|计算机配置 \ 管理模板 \windows 组件 \ Components\Biometrics|通常，这些策略设置允许或拒绝使用生物识别作为身份验证方法。<br /><br />有关 Windows 实现的生物识别信息，请参阅 Windows Biometric Framework 概述。|
|**凭据用户界面**|计算机配置 \ 管理模板 \windows 组件 \ Components\Credential 用户界面|这些策略设置控制凭据在入口点的管理方式。|
|**密码同步**|计算机配置 \ 管理模板 \windows 组件 \ Components\Password 同步|这些策略设置确定如何在系统管理的 Windows 和基于 UNIX 的操作系统之间的密码同步。<br /><br />有关详细信息，请参阅[密码同步](https://technet.microsoft.com/library/cc732609.aspx)。|
|**智能卡**|Computer Configuration\Administrative Templates\Windows Components\Smart Card|这些策略设置控制如何在系统管理智能卡登录。<br /><br />|
|**Windows 登录选项**|计算机配置 \ 管理模板 \windows 组件 \windows 登录选项|这些策略设置控制何时以及如何登录机会可用。|
|**Ctrl + Alt + Del 选项**|计算机配置 \ 管理模板 \windows 组件 \ Components\Ctrl + Alt + Del 选项|这些策略设置影响的外观和功能上登录 UI （安全桌面），如任务管理器和键盘锁定的计算机的可访问性。|
|**登录**|Computer Configuration\Administrative Templates\Windows Components\Logon|这些策略设置确定如果或当用户登录时，可以运行的进程。|

## <a name="see-also"></a>请参阅
[Windows 身份验证技术概述](windows-authentication-technical-overview.md)


