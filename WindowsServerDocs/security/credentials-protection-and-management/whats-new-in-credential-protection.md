---
title: 凭据保护的新增功能
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2351be82ad1d8b9af17715ce363836f57c71ea66
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386921"
---
# <a name="whats-new-in-credential-protection"></a>凭据保护的新增功能

## <a name="credential-guard-for-signed-in-user"></a>已登录用户的 Credential Guard

从 Windows 10 版本1507开始，Kerberos 和 NTLM 使用基于虚拟化的安全性来保护已登录用户登录会话的 Kerberos & NTLM 机密。 

从 Windows 10 版本1511开始，凭据管理器使用基于虚拟化的安全性来保护域凭据类型的已保存凭据。 使用远程桌面时，登录凭据和保存的域凭据不会传递到远程主机。 无需 UEFI 锁定即可启用 Credential Guard。

从 Windows 10 开始，版本1607，独立用户模式包含在 Hyper-v 中，因此不会再为 Credential Guard 部署单独安装它。

[详细了解 Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)。


## <a name="remote-credential-guard-for-signed-in-user"></a>已登录用户的远程 Credential Guard

从 Windows 10 版本1607开始，在使用远程桌面时，可通过保护客户端设备上的 Kerberos 和 NTLM 机密来保护已登录的用户凭据。 为了使远程主机能够作为用户评估网络资源，身份验证请求要求客户端设备使用机密。

从 Windows 10 版本1703开始，使用远程桌面时，远程 Credential Guard 会保护提供的用户凭据。

[详细了解远程 credential guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)。

## <a name="domain-protections"></a>域保护

域保护需要 Active Directory 域。

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>已加入域的设备支持使用公钥进行身份验证

从 Windows 10 版本1507和 Windows Server 2016 开始，如果已加入域的设备能够向 Windows Server 2016 域控制器（DC）注册其绑定的公钥，则设备可以使用 Kerberos PKINIT 通过公钥进行身份验证Windows Server 2016 DC 的身份验证。

从 Windows Server 2016 开始，Kdc 支持使用 Kerberos 密钥信任进行身份验证。  

若[要详细了解 & Kerberos 密钥信任，已加入域的设备的公钥支持](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="pkinit-freshness-extension-support"></a>PKINIT 新鲜度扩展支持

从 Windows 10 版本1507和 Windows Server 2016 开始，Kerberos 客户端将为基于公钥的登录尝试 PKInit 新鲜度扩展。 

从 Windows Server 2016 开始，Kdc 可以支持 PKInit 新鲜度扩展。  默认情况下，Kdc 将不提供 PKInit 新鲜度扩展。 

[详细了解 PKINIT 新鲜度 extension 支持](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>仅滚动公钥用户的 NTLM 机密

从 Windows Server 2016 域功能级别（DFL）开始，Dc 可支持仅滚动使用公钥的用户 NTLM 机密。 此功能在 DFLs 较低的 unavailble。

> [!WARNING] 
> 使用至少11月8日2016服务将域控制器添加到已启用滚动的 NTLM 密钥的域后，将会运行 DC 崩溃的风险。 

配置：对于新域，默认情况下启用此功能。 对于现有域，必须在 Active Directory 管理中心进行配置： 

1. 在 Active Directory 管理中心，右键单击左窗格中的域，然后选择 "**属性**"。

    ![域属性](../media/Credentials-Protection-And-Management/domain-properties.png)

2. **对于需要使用 Microsoft Passport 或智能卡进行交互式登录的用户，请选择 "在登录期间启用过期的 NTLM 机密"** 。

    ![Autoroll 过期 NTLM 机密](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. 单击 **“确定”** 。 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>当用户仅限于特定的已加入域的设备时允许网络 NTLM

从 Windows Server 2016 域功能级别（DFL）开始，如果用户仅限于特定的已加入域的设备，DCs 就可以支持允许网络 NTLM。 此功能在较低的 DFLs 中不可用。

配置：在 "身份验证策略" 上，单击 **"允许将 NTLM 网络身份验证限制为所选设备"** 。 

[详细了解身份验证策略](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos)。
