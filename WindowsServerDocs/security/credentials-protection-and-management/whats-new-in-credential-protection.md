---
title: "什么是凭据保护中的新增功能"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 0556c606b987a69eae663b0196467f532d5a307a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-credential-protection"></a>什么是凭据保护中的新增功能

## <a name="credential-guard-for-signed-in-user"></a>对于已登录的用户 Credential Guard

从 Windows 10 版本 1507，开始 Kerberos 和 NTLM 使用虚拟化基于安全保护已登录的用户登录会话的 Kerberos 和 NTLM 机密信息。 

从 Windows 10 版本 1511，开始凭据管理器使用虚拟化基于安全保护域凭据类型的已保存的凭据。 在登录凭据和保存的域凭据不会传递到远程使用远程桌面主机。 不 UEFI 锁定的情况下，可启用 Credential Guard。

从 Windows 10 版本 1607，开始独立用户模式是 Hyper-V 包含以便不再单独安装它以供进行 Credential Guard 部署。

[了解有关 Credential Guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/credential-guard)。


## <a name="remote-credential-guard-for-signed-in-user"></a>已登录的用户的远程 Credential Guard

开始使用 Windows 10 版本 1607，远程 Credential Guard 保护的用户在登录凭据保护 Kerberos 和 NTLM 机密信息在客户端的设备上使用远程桌面时。 远程评估网络资源为用户的主机、身份验证请求需要客户端设备使用机密信息。

Windows 10 版本 1703、开头远程 Credential Guard 保护的用户提供的凭据时使用远程桌面。

[了解有关远程凭据 guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/remote-credential-guard)。

## <a name="domain-protections"></a>域保护

域保护措施需 Active Directory 域。

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>已加入域的设备支持使用公钥进行身份验证

开始使用 Windows 10 版本 1507 年和 Windows Server 2016，，已加入域的设备是否可以将其绑定的公钥注册 Windows Server 2016 域控制器 (DC)，然后设备可以使用 Windows Server 2016 DC Kerberos PKINIT 身份验证的公钥进行身份验证。

Windows Server 2016 的开始 Kdc 支持使用 Kerberos 关键信任身份验证。  

[了解有关公共主要支持加入域的设备和 Kerberos 关键信任](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="pkinit-freshness-extension-support"></a>新鲜 PKINIT 扩展支持

从 Windows 10 版本 1507 年和 Windows Server 2016 开始，Kerberos 客户端尝试公共关键基于登录加载项 PKInit 新鲜扩展。 

Windows Server 2016 的开始 Kdc 可支持 PKInit 新鲜扩展。  默认情况下，Kdc 不会提供 PKInit 新鲜扩展。 

[了解有关 PKINIT 新鲜扩展支持](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>推出公钥仅用户的 NTLM 机密

从 Windows Server 2016 域功能级别 (DFL) 开始，域控制器可支持推出公钥仅用户的 NTLM 机密信息。 此功能非常低 DFLs 中的用。

> [!WARNING] 
> 添加到某个域，与推出前 DC 已更新为至少 2016 年 11 月 8 日服务运行启用 NTLM 机密 DC 崩溃的风险域控制器。 

配置：对于新的域中，此功能处于默认启用。 对于现有域，必须将其配置 Active Directory 管理中心中： 

1. 从 Active Directory 管理中心中，右键单击的域左侧窗格中，选择**属性**。

    ![域属性](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. 选择**启用推出的过期 NTLM 机密信息在登录，需要使用 Microsoft Passport 或智能卡登录的用户**。

    ![Autoroll 到期 NTLM 机密信息](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. 单击**确定**。 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>允许网络 NTLM 何时用户限定为特定加入域的设备

从 Windows Server 2016 域功能级别 (DFL) 开始，域控制器可支持允许网络 NTLM 用户时限制为特定加入域的设备。 不适用于较低 DFLs 此功能。

配置：在身份验证的策略中，单击**允许 NTLM 网络身份验证时用户仅限于选定设备**。 

[了解身份验证策略](https://technet.microsoft.com/en-us/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos)。
