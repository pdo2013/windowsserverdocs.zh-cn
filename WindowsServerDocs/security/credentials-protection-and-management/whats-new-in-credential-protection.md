---
title: 什么是凭据保护中的新增功能
description: Windows Server 安全
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
ms.openlocfilehash: ec41e85949cb61c8130d8765b4786eefe39ebd0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855588"
---
# <a name="whats-new-in-credential-protection"></a>什么是凭据保护中的新增功能

## <a name="credential-guard-for-signed-in-user"></a>已登录用户的 Credential Guard

从 Windows 10 版本 1507 但开始 Kerberos 和 NTLM 使用基于虚拟化的安全保护 Kerberos 和 NTLM 机密登录的用户登录会话。 

从 Windows 10 版本 1511，开始凭据管理器使用基于虚拟化的安全性来保护域凭据类型的已保存的凭据。 登录的凭据和已保存的域凭据不能传递到远程主机使用远程桌面。 可以无 UEFI 锁启用 Credential Guard。

从 Windows 10，版本 1607 中，开始隔离用户模式是附带的 HYPER-V 使它不再单独安装 Credential Guard 部署。

[了解有关 Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)。


## <a name="remote-credential-guard-for-signed-in-user"></a>有关已登录用户的远程 Credential Guard

使用远程桌面来保护客户端设备上的 Kerberos 和 NTLM 机密时，从 Windows 10，版本 1607 中，开始远程 Credential Guard 保护已登录用户凭据。 远程主机作为用户进行评估网络资源，身份验证请求需要客户端设备能够使用机密。

使用远程桌面时，从 Windows 10，版本 1703，开始远程 Credential Guard 保护提供的用户凭据。

[要详细了解远程 credential guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)。

## <a name="domain-protections"></a>域保护

域保护需要 Active Directory 域。

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>使用公钥身份验证的已加入域的设备支持

如果已加入域的设备能够注册其绑定的公钥和 Windows Server 2016 的域控制器 (DC) 从 Windows 10 版本 1507年和 Windows Server 2016，然后设备使用进行身份验证使用 Kerberos PKINIT 的公钥对 Windows Server 2016 DC 身份验证。

从 Windows Server 2016 开始，Kdc 支持使用 Kerberos 密钥信任身份验证。  

[要详细了解有关已加入域的设备和 Kerberos 密钥信任的公共密钥支持](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="pkinit-freshness-extension-support"></a>PKINIT 新鲜度扩展支持

从 Windows 10，版本 1507年和 Windows Server 2016 开始，Kerberos 客户端将尝试适用于公共密钥基于登录的 PKInit 新鲜度扩展。 

从 Windows Server 2016 开始，Kdc 可支持 PKInit 新鲜度扩展。  默认情况下，Kdc 将不提供对 PKInit 新鲜度扩展。 

[要详细了解 PKINIT 新鲜度扩展支持](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>滚动公共密钥唯一用户的 NTLM 机密

从 Windows Server 2016 域功能级别 (DFL) 开始，Dc 可以支持滚动公共密钥唯一用户的 NTLM 机密。 此功能是在较低 DFLs 不可。

> [!WARNING] 
> 将域控制器添加到域，并具有滚动 DC 已将更新为至少 8 2016 年 11 月服务运行之前启用 NTLM 机密的 DC 发生故障的风险。 

配置：对于新的域，默认情况下启用此功能。 对于现有的域，它必须配置 Active Directory 管理的中心： 

1. 从 Active Directory Administrative center 中，右键单击左窗格上的域，然后选择**属性**。

    ![域属性](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. 选择**启用滚动的即将到期的 NTLM 机密在登录过程，需要使用 Microsoft Passport 或智能卡进行交互式登录的用户的**。

    ![Autoroll 即将到期的 NTLM 机密](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. 单击 **“确定”**。 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>用户限制到特定的已加入域的设备时允许网络 NTLM

如果某个用户是限于特定的已加入域的设备，开头的 Windows Server 2016 域功能级别 (DFL) Dc 可以支持允许网络 NTLM。 此功能是在较低 DFLs 中不可用。

配置：在身份验证策略中，单击**允许 NTLM 网络身份验证时用户仅限于所选设备**。 

[了解有关身份验证策略的详细信息](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos)。
