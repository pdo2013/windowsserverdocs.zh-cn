---
title: "在 Windows 身份验证使用组策略设置"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>在 Windows 身份验证使用组策略设置

>适用于：Windows Server（半年通道），Windows Server 2016

IT 专业人员的此参考主题介绍了使用和验证过程中的组策略设置的影响。

通过添加用户、 计算机和服务帐户到组中，然后将应用到这些组策略身份验证，你可以管理身份验证 Windows 操作系统中。 随着本地安全策略管理模板，也称为组策略设置以及定义这些的策略。 这两组可以配置，并且可以使用组策略遍布你的组织。

> [!NOTE]
> Windows Server 2012 R2 中, 引入的功能，可配置认证策略为有针对性的服务，或通常使用名为身份验证思的应用程序受保护的帐户。 有关如何执行此操作 Active Directory 中的信息，请参阅[如何配置保护帐户](how-to-configure-protected-accounts.md)。

例如，你可以适用下面的策略于组、 基于在组织中的功能：

-   在本地或到某个域登录

-   在网络上登录

-   重置帐户

-   创建帐户

下表列出了策略组相关的身份验证，并提供指向可以帮助你的文档配置这些策略。

|组策略|位置|描述|
|--------|------|--------|
|**密码策略**|本地计算机策略配置 windows 设置 Settings\Account 策略|密码策略影响特征和密码的行为。 密码策略适用于域帐户或本地用户帐户。 它们决定密码，如执法和生命周期内的设置。<br /><br />有关特定设置的信息，请参阅[密码策略](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy)。|
|**帐户锁定策略**|本地计算机策略配置 windows 设置 Settings\Account 策略|帐户锁定策略选项禁用在多次登录尝试失败后帐户。 使用这些选项可帮助你检测并阻止尝试中断密码。<br /><br />有关帐户锁定策略选项的信息，请参阅[帐户锁定策略](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy)。|
|**Kerberos 策略**|本地计算机策略配置 windows 设置 Settings\Account 策略|Kerberos 相关的设置，包括票证生命周期内和强制规则。 Kerberos 策略并非适用于本地帐户数据库因为 Kerberos 身份验证协议不用于验证本地帐户。 因此，仅通过组策略对象 (GPO)，它会域登录的影响的默认域可以配置 Kerberos 策略设置。<br /><br />有关域控制器 Kerberos 策略选项的信息，请参阅[Kerberos 策略](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy)。|
|**审核策略**|本地计算机策略配置 windows 设置本地审核策略|审核策略允许您控制和了解到的对象、 文件和文件夹，例如以及管理用户和组帐户的用户登录和注销访问。 审核策略可以指定你希望审核、 设置的大小和行为的安全日志中，并确定你想要监控的访问权限的对象的并且想要监控哪种类型的访问权限的事件的类别。<br /><br />|
|**用户版权分配**|本地计算机策略配置 windows 设置本地策略用户版权分配|安全所属的组的用户，例如管理员、 的超级用户，或者用户的基础，通常分配用户权限。 在此类别中的策略设置通常用于授予或拒绝基于访问和安全性的组成员方法计算机的访问权。|
|**安全选项**|本地计算机策略配置 windows 设置本地安全选项|相关的身份验证的策略包括：<br /><br />-设备<br />-域控制器<br />域成员<br />-交互式登录<br />Microsoft 网络服务器<br />的网络访问权限<br />网络安全<br />恢复主机<br />关闭<br /><br />|
|**凭据委派**|计算机配置 Templates\System\Credentials 委派|凭据委派是一种可使其他系统，尤其是成员服务器而内域域控制器上使用的本地凭据机制。 这些设置适用于应用通过使用凭据安全支持供应商 (凭据 SSP)。 远程桌面连接是一个示例。|
|**KDC**|计算机配置 Templates\System\KDC|这些的策略设置会影响密钥 Distribution 中心 (KDC)，这是在该域控制器上的服务，如何处理 Kerberos 身份验证的请求。|
|**Kerberos**|计算机配置 Templates\System\Kerberos|这些的策略设置会影响如何配置 Kerberos 以处理索赔、 Kerberos 程度、 复合身份验证、 识别的代理服务器和其他配置支持。|
|**登录**|计算机配置登录|这些的策略设置来控制如何系统会显示为用户登录的体验。|
|**网络登录**|计算机配置 Templates\System\Net 登录|这些的策略设置来控制系统处理网络登录请求，包括域控制器定位器行为方式的方式。<br /><br />有关如何域控制器定位器适应复制进程的详细信息，请参阅[了解之间站点上的复制](https://technet.microsoft.com/library/cc771251.aspx)。|
|**生物识别**|计算机配置 \windows Components\Biometrics|通常，这些的策略设置允许，或拒绝身份验证方法为使用生物识别。<br /><br />有关 Windows 中执行的生物识别的信息，请参阅 Windows 生物框架概述。|
|**凭据用户界面**|计算机配置 \windows Components\Credential 用户界面|这些的策略设置来控制凭据条目可以管理的方式。|
|**密码同步**|计算机配置 \windows Components\Password 同步|这些的策略设置确定如何系统管理密码 Windows 和 UNIX 操作系统之间同步。<br /><br />有关详细信息，请参阅[密码同步](https://technet.microsoft.com/library/cc732609.aspx)。|
|**智能卡**|计算机配置 \windows Components\Smart 卡|这些的策略设置来控制系统管理智能卡登录的方式。<br /><br />|
|**Windows 登录选项**|计算机配置 \windows \windows 登录选项|这些的策略设置控制何时以及如何登录机会可用。|
|**Ctrl + Alt + Del 选项**|计算机配置 \windows Components\Ctrl + Alt + Del 选项|这些的策略设置会影响的外观和辅助功能在登录 UI （安全桌面） 上，如任务管理器和计算机的键盘锁定。|
|**登录**|计算机配置 \windows Components\Logon|这些的策略设置确定是否或用户在登录时，可以运行的进程。|

## <a name="see-also"></a>请参阅
[Windows 身份验证 Technical 概述](windows-authentication-technical-overview.md)


