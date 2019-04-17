---
title: "受保护的用户安全组"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bd6b53c0febdfb2d344136097a9654c981405568
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="protected-users-security-group"></a>受保护的用户安全组

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员描述 Active Directory 安全组受保护的用户，并解释它的工作原理。 Windows Server 2012 R2 域控制器在引入了此组。

## <a name="BKMK_ProtectedUsers"></a>概述

安全该组作为策略的一部分旨在管理凭据暴露在企业中。 此组成员自动具有非配置保护措施应用到他们的帐户。 保护用户组中的成员旨在严格和默认情况下主动保护。 修改这些保护帐户的唯一方法是删除帐户安全组中。

> [!WARNING]
> 帐户的服务和计算机绝不应保护用户组中的成员。 此组会提供仍不完整的保护，由于密码或证书始终在主机上可用。 身份验证将失败并错误 \"the 用户名和密码 incorrect\"的任何服务或添加到保护用户组中的计算机。

此域相关、全球组与运行 Windows Server 2012 R2 的主域控制器触发非配置保护设备和主计算机运行的 Windows Server 2012 R2 和 Windows 8.1 或更高版本的域中的用户。 当用户登录到计算机与这些保护，这大大减少对默认内存占用量造成的凭据。

有关详细信息，请参阅[保护用户如何组适用](#BKMK_HowItWorks)本主题中。



## <a name="BKMK_Requirements"></a>受保护的用户组要求
要求提供保护用户组成员的设备提供保护包括：

- 保护用户全球安全组复制到域帐户中的所有域控制器上。

- Windows 8.1 和 Windows Server 2012 R2 默认添加的支持。 [Microsoft 安全公告 2871997](https://technet.microsoft.com/library/security/2871997)将支持添加到 Windows 7、Windows Server 2008 R2 和 Windows Server 2012。

要求提供的保护用户组成员的域控制器保护包括：

- 用户必须是 Windows Server 2012 R2 或更高版本的域功能级别域中。

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>向低级别域添加保护用户全局安全组

运行操作系统早于 Windows Server 2012 R2 域控制器可支持添加到新的受保护的用户安全组的成员。 这使用户域升级前享受设备提供保护。 

> [!Note]
> 域控制器中将不支持域保护措施。 

可通过创建受保护的用户组[传输主域控制器 (PDC) 模拟器角色](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)到运行 Windows Server 2012 R2 域控制器。 该组对象复制到其他域控制器后，可以运行较早版本的 Windows Server 的域控制器上托管 PDC 模拟器角色。

### <a name="BKMK_ADgroup"></a>受保护的用户组广告属性

下表指定保护用户组的属性。

|属性|值|
|-------|-----|
|已知的 SID RID|S-1-5 个 21-<domain>-525|
|键入|全球域|
|默认容器|CN = 用户，直流 =<domain>，直流 =|
|默认成员|无|
|默认成员|无|
|通过 ADMINSDHOLDER 是否受到保护？|不|
|若要退出默认容器移动安全？|是的|
|若要管理该组到非服务管理员委托安全？|不|
|默认用户权限|没有默认用户权限|

## <a name="BKMK_HowItWorks"></a>保护用户组的工作原理
此部分中介绍了如何保护用户组工作时：

-   在 Windows 设备上登录

-   用户帐户域是在 Windows Server 2012 R2 或更高版本的域功能级别

### <a name="device-protections-for-signed-in-protected-users"></a>在受保护的用户登录的设备提供保护
保护用户组成员的用户在登录时以下保护措施应用：

-   凭据委派 (CredSSP) 将无法缓存的用户纯文本凭据即使**允许委派默认凭据**组策略设置。

-   开始使用 Windows 8.1 和 Windows Server 2012 R2、Windows 摘要将不缓存的用户纯文本的凭据 Windows 摘要处于启用状态，即使。

> [!Note]
> 安装之后[Microsoft 安全公告 2871997](https://technet.microsoft.com/library/security/2871997) Windows 摘要将继续缓存凭据直到配置注册表项。 请参阅[Microsoft 安全公告：更新以改进都凭据保护和管理：2014 年 5 月 13 日](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a)的说明进行操作。

-   NTLM 将缓存用户纯文本凭据或 NT 单向函数 (NTOWF)。

-   Kerberos 将不会再创建 DES 或 RC4 键。 此外它不会初始 TGT 获取之后中缓存用户纯文本凭据或长期键。

-   缓存的验证程序不会在登录且创建或解锁，因此不会再支持离线状态下登录。

用户帐户将添加到保护用户组后，用户登录到设备时，会开始保护。

### <a name="domain-controller-protections-for-protected-users"></a>对于受保护的用户域控制器保护
找不到所的组成员的受保护的用户，对 Windows Server 2012 R2 域进行身份验证的客户：

-   与 NTLM 身份验证，验证身份。

-   使用预 Kerberos 的身份验证 DES 或 RC4 加密类型。

-   使用不受约束或受限制委派委派。

-   续订 Kerberos Tgt 之外的初始四小时生命周期。

为 Tgt 过期非配置设置为每个帐户建立保护用户组中。 通常，域控制器设置 Tgt 生命周期内和续订，根据域的策略，**票证用户最长寿命**和**最长用户票证续订寿命**。 对于受保护的用户组中，600 分钟这些域策略设置。

有关详细信息，请参阅[如何配置保护帐户](how-to-configure-protected-accounts.md)。

## <a name="troubleshooting"></a>故障排除
两个操作管理日志，可帮助解决事件相关的受保护的用户。 这些新日志位于事件查看器中默认情况下，已禁用和位于下**应用程序和服务 Logs\ Microsoft \Windows\ Microsoft \Authentication**。

|事件 ID，并登录|描述|
|----------|--------|
|104<br /><br />**ProtectedUser 客户端**|原因：安全程序包客户端上的不包含的凭据。<br /><br />保护用户安全组成员的帐户时，将在客户端计算机记录错误。 此事件表示安全包不缓存需要为服务器身份验证的凭据。<br /><br />显示程序包名称、用户的名称、域名称和服务器的名称。|
|304<br /><br />**ProtectedUser 客户端**|原因：安全包不会存储受保护的用户的凭据。<br /><br />信息的事件将记录在客户端，以指示安全包不缓存用户的登录凭据。 预计，摘要 (WDigest)、委派凭据 (CredSSP)，以及 NTLM 无法已登录凭据的受保护的用户。 如果这些提示输入凭据，仍可以成功的应用程序。<br /><br />显示包姓名、用户名和域名。|
|100<br /><br />**ProtectedUserFailures 域控制器**|原因：NTLM 登录失败时发生保护用户安全组中的帐户。<br /><br />错误已登录的域控制器，以指示 NTLM 身份验证失败，因为该帐户已受到保护用户安全组成员。<br /><br />显示的帐户名称和设备名称。|
|104<br /><br />**ProtectedUserFailures 域控制器**|原因：DES 或 RC4 加密类型用于 Kerberos 身份验证，并保护用户安全组中的用户出现登录失败。<br /><br />Kerberos 预身份验证失败，因为保护用户安全组成员的帐户时，不能使用 DES 和 RC4 加密的类型。<br /><br />（AES 是可接受。）|
|303<br /><br />**ProtectedUserSuccesses 域控制器**|原因：的保护用户组成员成功发出 Kerberos-授权-票证 (TGT)。|



## <a name="additional-resources"></a>更多资源

-   [凭据保护和管理](credentials-protection-and-management.md)

-   [身份验证的策略和身份验证策略思](authentication-policies-and-authentication-policy-silos.md)

-   [如何配置受保护的帐户](how-to-configure-protected-accounts.md)


