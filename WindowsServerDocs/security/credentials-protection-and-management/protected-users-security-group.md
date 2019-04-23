---
title: 受保护的用户安全组
description: Windows Server 安全
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862358"
---
# <a name="protected-users-security-group"></a>受保护的用户安全组

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题面向 IT 专业人员介绍 Active Directory 安全组受保护用户，并说明工作原理。 Windows Server 2012 R2 的域控制器中引入了此组。

## <a name="BKMK_ProtectedUsers"></a>概述

此安全组设计为策略的一部分来管理企业中的凭据公开。 此组的成员将自动具有应用于其帐户的非可配置保护。 在默认情况下，受保护的用户组中的成员身份意味着受到限制并主动保护。 修改这些帐户保护的唯一方法是从安全组中删除该帐户。

> [!WARNING]
> 服务和计算机帐户不应是受保护用户组的成员。 此组仍将提供不完整的保护，因为密码或证书始终在主机上可用。 身份验证将失败并出现错误\"用户名或密码不正确\"任何服务或计算机添加到受保护用户组。

此域相关的全局组主域控制器运行 Windows Server 2012 R2 将触发不可配置的保护设备和主机计算机运行 Windows Server 2012 R2 和 Windows 8.1 或更高版本的域中的用户。 这极大地减少了凭据的默认内存占用，当用户登录到具有这些保护的计算机。

有关详细信息，请参阅[如何受保护用户组的工作原理](#BKMK_HowItWorks)本主题中。



## <a name="BKMK_Requirements"></a>受保护的用户组的要求
若要提供受保护用户组的成员的设备保护的要求包括：

- 受保护用户全局安全组被复制到帐户域中的所有域控制器。

- Windows 8.1 和 Windows Server 2012 R2 默认情况下添加支持。 [Microsoft 安全公告 2871997](https://technet.microsoft.com/library/security/2871997)向 Windows 7、 Windows Server 2008 R2 和 Windows Server 2012 的支持。

为受保护用户组的成员提供域控制器保护的要求包括：

- 用户必须是 Windows Server 2012 R2 或更高版本的域功能级别的域中。

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>将受保护用户全局安全组添加到下层域

运行的操作系统早于 Windows Server 2012 R2 的域控制器可支持添加到新的受保护用户安全组的成员。 这样用户就可以升级域之前从设备保护功能中受益。 

> [!Note]
> 域控制器将不支持域保护。 

可以通过创建受保护的用户组[主域控制器 (PDC) 模拟器角色传输](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)到运行 Windows Server 2012 R2 的域控制器。 将该组对象复制到其他域控制器后，PDC 模拟器角色可以托管在运行较早版本的 Windows Server 的域控制器上。

### <a name="BKMK_ADgroup"></a>受保护的用户组 AD 属性

下表指定受保护的用户组的属性。

|特性|ReplTest1|
|-------|-----|
|已知 SID/RID|S-1-5-21-<domain>-525|
|在任务栏的搜索框中键入|域全局|
|默认容器|CN=Users，DC=<domain>，DC=|
|默认成员|无|
|默认成员|无|
|通过 ADMINSDHOLDER 受保护吗？|否|
|移出默认容器是否安全？|是|
|将此组的管理委派给非服务管理员是否安全？|否|
|默认用户权限|没有默认的用户权限|

## <a name="BKMK_HowItWorks"></a>受保护用户组的工作原理
本部分介绍在以下情况下受保护的用户组的工作原理：

-   在 Windows 设备的登录

-   用户帐户域是在 Windows Server 2012 R2 或更高版本的域功能级别

### <a name="device-protections-for-signed-in-protected-users"></a>在受保护的用户的登录针对的设备保护
如果已登录用户是受保护用户组的成员会应用以下保护：

-   凭据委派 (CredSSP) 将缓存用户的纯文本凭据，即使**允许委派默认凭据**启用组策略设置。

-   从 Windows 8.1 和 Windows Server 2012 R2 开始，Windows 摘要不会缓存用户的纯文本凭据即使在启用了 Windows Digest。

> [!Note]
> 安装后[Microsoft 安全公告 2871997](https://technet.microsoft.com/library/security/2871997) Windows 摘要将继续到缓存的凭据，直到配置注册表项。 请参阅[Microsoft 安全公告：更新后改进凭据保护和管理：2014 年 5 月 13，](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a)有关的说明。

-   NTLM 不会缓存用户的纯文本凭据或 NT 单向函数 (NTOWF)。

-   Kerberos 将无法再创建 DES 或 RC4 密钥。 此外它不会获得初始 TGT 后中缓存用户的纯文本凭据或长期密钥。

-   缓存验证程序不会创建在登录或解锁，因此不再支持脱机登录。

用户帐户添加到受保护用户组后，当用户登录到设备时，将开始保护。

### <a name="domain-controller-protections-for-protected-users"></a>为受保护用户的域控制器保护
向 Windows Server 2012 R2 域进行身份验证的受保护用户组成员的帐户不能为：

-   使用 NTLM 身份验证进行验证。

-   在 Kerberos 预身份验证中使用 DES 或 RC4 加密类型。

-   使用不受约束或约束的委派进行委派。

-   在超出最初的四小时生存期后续订 Kerberos TGT。

在受保护的用户组中为每个帐户建立 TGT 到期的非可配置设置。 通常，域控制器基于域策略、“用户票证最长生存期”和“用户票证续订的最长生存期”设置 TGT 生存期和续订。 对于受保护的用户组，为这些域策略设置为 600 分钟。

有关详细信息，请参阅[如何配置受保护的帐户](how-to-configure-protected-accounts.md)。

## <a name="troubleshooting"></a>疑难解答
提供两个操作管理日志，以帮助对受保护用户的相关事件进行疑难解答。 这些新的日志位于事件查看器中，在默认情况下已禁用，并且位于“Applications and Services Logs\Microsoft\Windows\Microsoft\Authentication” 下。

|事件 ID 和日志|描述|
|----------|--------|
|104<br /><br />“受保护用户客户端”|原因：客户端上的安全程序包不包含这些凭据。<br /><br />当该帐户是受保护的用户安全组的成员时，将在客户端计算机中记录错误。 此事件指示安全程序包不会缓存在对服务器进行身份验证时所需的凭据。<br /><br />显示程序包名称、用户名、域名和服务器名称。|
|304<br /><br />“受保护用户客户端”|原因：安全程序包不会存储受保护用户的凭据。<br /><br />信息事件记录在客户端来指示安全程序包不会缓存用户的登录凭据。 预期结果是 Digest (WDigest)、凭据委派 (CredSSP) 和 NTLM 无法具有受保护用户的登录凭据。 如果提示输入凭据，则仍然能够成功执行应用程序。<br /><br />显示程序包名称、用户名和域名。|
|100<br /><br />**ProtectedUserFailures-DomainController**|原因：对于在受保护的用户安全组中的帐户，发生 NTLM 登录失败。<br /><br />在域控制器中记录错误，以指示 NTLM 身份验证失败，因为该帐户已是受保护用户安全组的成员。<br /><br />显示帐户名称和设备名称。|
|104<br /><br />**ProtectedUserFailures-DomainController**|原因：DES 或 RC4 加密类型用于进行 Kerberos 身份验证，以及对于受保护用户安全组中的用户，发生登录故障。<br /><br />Kerberos 预身份验证失败，因为当该帐户是受保护用户安全组的成员时，不能使用 DES 和 RC4 加密类型。<br /><br />（AES 是可接受的。）|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|原因：对于受保护用户组的成员，已成功进行 Kerberos 票证授予票证 (TGT)。|



## <a name="additional-resources"></a>其他资源

-   [凭据保护和管理](credentials-protection-and-management.md)

-   [身份验证策略和身份验证策略接收器](authentication-policies-and-authentication-policy-silos.md)

-   [如何配置受保护的帐户](how-to-configure-protected-accounts.md)


