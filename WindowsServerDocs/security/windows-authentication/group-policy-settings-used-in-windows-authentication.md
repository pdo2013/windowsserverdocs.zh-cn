---
title: Windows 身份验证中使用的组策略设置
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 352e67dead6e22085a8e7350dd7e5c18c6d74676
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403255"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Windows 身份验证中使用的组策略设置

>适用于：Windows Server（半年频道）、Windows Server 2016

本参考主题面向 IT 专业人员，介绍了身份验证过程中组策略设置的使用和影响。

你可以通过将用户、计算机和服务帐户添加到组，然后将身份验证策略应用于这些组，来管理 Windows 操作系统中的身份验证。 这些策略定义为本地安全策略和管理模板，也称为组策略设置。 通过使用组策略，可以在整个组织中配置和分发这两个集。

> [!NOTE]
> Windows Server 2012 R2 中引入的功能使你可以使用受保护的帐户为目标服务或应用程序（通常称为身份验证接收器）配置身份验证策略。 有关如何在 Active Directory 中执行此操作的信息，请参阅[如何配置受保护的帐户](how-to-configure-protected-accounts.md)。

例如，你可以根据组织中的功能，将以下策略应用于组：

-   本地登录或登录到域

-   通过网络登录

-   重置帐户

-   创建帐户

下表列出了与身份验证相关的策略组，并提供指向可帮助你配置这些策略的文档的链接。

|策略组|Location|描述|
|--------|------|--------|
|**密码策略**|本地计算机策略 \dns 配置 \Windows 设置 \ 安全设置 Settings\Account 策略|密码策略影响密码的特征和行为。 密码策略用于域帐户或本地用户帐户。 它们确定密码设置，如强制和生存期。<br /><br />有关特定设置的信息，请参阅[密码策略](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy)。|
|**帐户锁定策略**|本地计算机策略 \dns 配置 \Windows 设置 \ 安全设置 Settings\Account 策略|帐户锁定策略选项在失败的登录尝试次数后禁用帐户。 使用这些选项可帮助检测和阻止破解密码的尝试。<br /><br />有关帐户锁定策略选项的信息，请参阅[帐户锁定策略](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy)。|
|**Kerberos 策略**|本地计算机策略 \dns 配置 \Windows 设置 \ 安全设置 Settings\Account 策略|与 Kerberos 相关的设置包括票证生存期和强制规则。 Kerberos 策略不适用于本地帐户数据库，因为 Kerberos 身份验证协议不用于对本地帐户进行身份验证。 因此，只能通过默认域组策略对象（GPO）来配置 Kerberos 策略设置，这会影响域登录。<br /><br />有关域控制器的 Kerberos 策略选项的信息，请参阅[Kerberos 策略](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy)。|
|**审核策略**|本地计算机策略 \dns 配置 \Windows 设置 \ 安全设置 \ 本地审核策略|审核策略可让你控制和了解对对象（如文件和文件夹）的访问权限，以及管理用户和组帐户以及用户登录和注销。 审核策略可以指定要审核的事件的类别，设置安全日志的大小和行为，并确定要监视其访问权限的对象以及要监视的访问类型。<br /><br />|
|**用户权限分配**|本地计算机策略 \dns 配置 \Windows 设置 \ 安全设置 \ 本地策略 \ 权限分配|用户权限通常基于用户所属的安全组（如管理员、超级用户或用户）进行分配。 此类别中的策略设置通常用于根据访问和安全组成员身份的方法授予或拒绝访问计算机的权限。|
|**安全选项**|本地计算机策略 \dns 配置 \Windows 设置 \ 安全设置 \ 安全选项|与身份验证相关的策略包括：<br /><br />-设备<br />-域控制器<br />-域成员<br />-交互式登录<br />-Microsoft 网络服务器<br />-网络访问<br />-网络安全<br />-恢复控制台<br />-Shutdown<br /><br />|
|**凭据委派**|计算机配置 \ 管理模板 Templates\System\Credentials 委派|凭据的委托是一种机制，允许在其他系统上使用本地凭据，最值得注意的是域中的成员服务器和域控制器。 这些设置适用于使用凭据安全支持提供程序（凭据 SSP）的应用程序。 远程桌面连接是一个示例。|
|**KDC**|计算机配置 \ 管理 Templates\System\KDC|这些策略设置会影响密钥发行中心（KDC）（即域控制器上的服务）处理 Kerberos 身份验证请求的方式。|
|**V5**|计算机配置 \ 管理 Templates\System\Kerberos|这些策略设置会影响如何配置 Kerberos 来处理对声明、Kerberos 保护、复合身份验证、标识代理服务器和其他配置的支持。|
|**登录**|计算机配置\管理模板\系统\登录|这些策略设置控制系统如何提供用户的登录体验。|
|**Net Logon**|计算机配置 \ 管理 Templates\System\Net 登录|这些策略设置控制系统如何处理网络登录请求，包括域控制器定位器的行为方式。<br /><br />有关域控制器定位程序如何适合复制过程的详细信息，请参阅[了解站点间的复制](https://technet.microsoft.com/library/cc771251.aspx)。|
|**指标**|计算机配置 \ 管理模板 \Windows 组件 Components\Biometrics|这些策略设置通常允许或拒绝使用生物识别作为身份验证方法。<br /><br />有关生物识别的 Windows 实现的信息，请参阅 Windows Biometric Framework 概述。|
|**凭据用户界面**|计算机配置 \ 管理模板 \Windows 组件 Components\Credential 用户界面|这些策略设置控制如何在入口点管理凭据。|
|**密码同步**|计算机配置 \ 管理模板 \Windows 组件 Components\Password 同步|这些策略设置确定系统如何管理基于 Windows 和 UNIX 的操作系统之间的密码同步。<br /><br />有关详细信息，请参阅[密码同步](https://technet.microsoft.com/library/cc732609.aspx)。|
|**智能卡**|计算机配置 \ 管理模板 \Windows 组件 Components\Smart 卡|这些策略设置控制系统管理智能卡登录的方式。<br /><br />|
|**Windows 登录选项**|计算机配置 \ 管理模板 \Windows 组件 \Windows 登录选项|这些策略设置控制登录机会的使用时间和方式。|
|**Ctrl + Alt + Del 选项**|计算机配置 \ 管理模板 \Windows 组件 Components\Ctrl + Alt + Del 选项|这些策略设置将影响登录 UI （安全桌面）上功能的外观和可访问性，如任务管理器和计算机的键盘锁定。|
|**登录**|计算机配置 \ 管理模板 \Windows 组件 Components\Logon|这些策略设置确定用户是否可以在用户登录时运行哪些进程。|

## <a name="see-also"></a>请参阅
[Windows 身份验证技术概述](windows-authentication-technical-overview.md)


