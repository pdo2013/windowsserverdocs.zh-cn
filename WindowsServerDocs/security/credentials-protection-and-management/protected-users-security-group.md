---
title: 受保护的用户安全组
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0cbec876ebf8a3ce27bf0d6f099ade6a5d6bc032
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403772"
---
# <a name="protected-users-security-group"></a>受保护的用户安全组

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题面向 IT 专业人员介绍 Active Directory 安全组受保护用户，并说明工作原理。 此组是在 Windows Server 2012 R2 域控制器中引入的。

## <a name="BKMK_ProtectedUsers"></a>叙述

此安全组设计为管理企业内凭据公开的策略的一部分。 此组的成员将自动具有应用于其帐户的非可配置保护。 在默认情况下，受保护的用户组中的成员身份意味着受到限制并主动保护。 修改这些帐户保护的唯一方法是从安全组中删除该帐户。

> [!WARNING]
> 服务和计算机的帐户不应是受保护用户组的成员。 由于主机上的密码或证书始终可用，此组仍将提供不完整的保护。 身份验证将失败，并出现错误 \"the 用户名或密码不正确，因为添加到受保护用户组的任何服务或计算机的用户名或密码不正确。

在运行 Windows Server 2012 R2 的设备和主机计算机上，此域相关的全局组将触发不可配置的保护，并且在具有运行 Windows Server 2012 R2 的主域控制器的域中的用户 Windows 8.1 或更高版本。 当用户登录到具有这些保护的计算机时，这会大大减少凭据的默认内存占用量。

有关详细信息，请参阅本主题中[的受保护用户组的工作原理](#BKMK_HowItWorks)。



## <a name="BKMK_Requirements"></a>受保护用户组要求
为受保护用户组的成员提供设备保护的要求包括：

- 受保护用户全局安全组被复制到帐户域中的所有域控制器。

- 默认情况下，Windows 8.1 和 Windows Server 2012 R2 已添加支持。 [Microsoft 安全公告 2871997](https://technet.microsoft.com/library/security/2871997)添加了对 windows 7、windows Server 2008 R2 和 windows server 2012 的支持。

为受保护用户组的成员提供域控制器保护的要求包括：

- 用户必须位于 Windows Server 2012 R2 或更高版本的域功能级别的域中。

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>向底层域添加受保护的用户全局安全组

运行早于 Windows Server 2012 R2 的操作系统的域控制器可以支持将成员添加到新的受保护用户安全组。 这允许用户在升级域之前从设备保护中获益。 

> [!Note]
> 域控制器将不支持域保护。 

可以通过将[主域控制器（PDC）模拟器角色传输](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)到运行 Windows Server 2012 R2 的域控制器来创建受保护的用户组。 将该组对象复制到其他域控制器后，PDC 模拟器角色可以托管在运行较早版本的 Windows Server 的域控制器上。

### <a name="BKMK_ADgroup"></a>受保护用户组的 AD 属性

下表指定受保护的用户组的属性。

|特性|ReplTest1|
|-------|-----|
|已知 SID/RID|S-1-5-21-<domain>-525|
|type|域全局|
|默认容器|CN=Users，DC=<domain>，DC=|
|默认成员|无|
|默认成员|无|
|通过 ADMINSDHOLDER 受保护吗？|否|
|移出默认容器是否安全？|是|
|将此组的管理委派给非服务管理员是否安全？|否|
|默认用户权限|没有默认的用户权限|

## <a name="BKMK_HowItWorks"></a>受保护用户组的工作原理
本部分介绍在以下情况下受保护的用户组的工作原理：

-   已登录到 Windows 设备

-   用户帐户域在 Windows Server 2012 R2 或更高版本的域功能级别中

### <a name="device-protections-for-signed-in-protected-users"></a>受保护用户登录的设备保护
当登录用户是受保护用户组的成员时，将应用以下保护：

-   即使启用了 "**允许委派默认凭据**" 组策略设置，凭据委托（CredSSP）也不会缓存用户的纯文本凭据。

-   从 Windows 8.1 和 Windows Server 2012 R2 开始，即使启用了 Windows Digest，Windows 摘要式也不会缓存用户的纯文本凭据。

> [!Note]
> 安装[Microsoft 安全公告 2871997](https://technet.microsoft.com/library/security/2871997)后，Windows 摘要将继续缓存凭据，直到配置了注册表项。 请参阅 [Microsoft 安全公告：更新以改进凭据保护和管理：2014年5月13日 @ no__t，说明。

-   NTLM 不会缓存用户的纯文本凭据或 NT 单向功能（NTOWF）。

-   Kerberos 将不再创建 DES 或 RC4 密钥。 此外，在获取初始 TGT 后，它不会缓存用户的纯文本凭据或长期密钥。

-   在登录或解锁时不会创建缓存的验证程序，因此不再支持脱机登录。

将用户帐户添加到受保护的用户组后，当用户登录到设备时，将开始保护。

### <a name="domain-controller-protections-for-protected-users"></a>受保护用户的域控制器保护
作为对 Windows Server 2012 R2 域进行身份验证的受保护用户组的成员的帐户无法执行以下操作：

-   使用 NTLM 身份验证进行验证。

-   在 Kerberos 预身份验证中使用 DES 或 RC4 加密类型。

-   使用不受约束或约束的委派进行委派。

-   在超出最初的四小时生存期后续订 Kerberos TGT。

在受保护的用户组中为每个帐户建立 TGT 到期的非可配置设置。 通常，域控制器基于域策略、“用户票证最长生存期”和“用户票证续订的最长生存期”设置 TGT 生存期和续订。 对于受保护的用户组，为这些域策略设置为 600 分钟。

有关详细信息，请参阅[如何配置受保护的帐户](how-to-configure-protected-accounts.md)。

## <a name="troubleshooting"></a>疑难解答
提供两个操作管理日志，以帮助对受保护用户的相关事件进行疑难解答。 这些新的日志位于事件查看器中，在默认情况下已禁用，并且位于“Applications and Services Logs\Microsoft\Windows\Microsoft\Authentication”下。

|事件 ID 和日志|描述|
|----------|--------|
|104<br /><br />“受保护用户客户端”|原因：客户端上的安全程序包不包含这些凭据。<br /><br />当该帐户是受保护的用户安全组的成员时，将在客户端计算机中记录错误。 此事件指示安全程序包不会缓存在对服务器进行身份验证时所需的凭据。<br /><br />显示程序包名称、用户名、域名和服务器名称。|
|304<br /><br />“受保护用户客户端”|原因：安全程序包不会存储受保护用户的凭据。<br /><br />将在客户端中记录信息性事件，以指示安全程序包不会缓存用户的登录凭据。 预期结果是 Digest (WDigest)、凭据委派 (CredSSP) 和 NTLM 无法具有受保护用户的登录凭据。 如果提示输入凭据，则仍然能够成功执行应用程序。<br /><br />显示程序包名称、用户名和域名。|
|100<br /><br />**ProtectedUserFailures-DomainController**|原因：对于在受保护的用户安全组中的帐户，发生 NTLM 登录失败。<br /><br />在域控制器中记录错误，以指示 NTLM 身份验证失败，因为该帐户已是受保护用户安全组的成员。<br /><br />显示帐户名称和设备名称。|
|104<br /><br />**ProtectedUserFailures-DomainController**|原因：DES 或 RC4 加密类型用于进行 Kerberos 身份验证，以及对于受保护用户安全组中的用户，发生登录故障。<br /><br />Kerberos 预身份验证失败，因为当该帐户是受保护用户安全组的成员时，不能使用 DES 和 RC4 加密类型。<br /><br />（AES 是可接受的。）|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|原因：对于受保护用户组的成员，已成功进行 Kerberos 票证授予票证 (TGT)。|



## <a name="additional-resources"></a>其他资源

-   [凭据保护和管理](credentials-protection-and-management.md)

-   [身份验证策略和身份验证策略接收器](authentication-policies-and-authentication-policy-silos.md)

-   [如何配置受保护的帐户](how-to-configure-protected-accounts.md)


