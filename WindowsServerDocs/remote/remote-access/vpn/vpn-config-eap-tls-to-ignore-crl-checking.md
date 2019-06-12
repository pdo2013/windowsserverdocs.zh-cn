---
title: 配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查
description: EAP-TLS 客户端无法连接，除非 NPS 服务器完成的客户端证书链 （包括根证书） 吊销检查并验证已吊销证书。
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 781239f45b9b260b7d374c2a6972cdb8faad2879
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749593"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>步骤 7.1： 配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

- [**上一：** 步骤 7：（可选）针对 VPN 连接使用 Azure AD 条件性访问](ad-ca-vpn-connectivity-windows10.md)
- [**下一步：** 步骤 7.2：使用 Azure AD 创建 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>如果无法实现此注册表更改将导致 IKEv2 连接使用与 PEAP 云证书失败，但使用从内部 CA 颁发的客户端身份验证证书的 IKEv2 连接将继续运行。

在此步骤中，您可以添加**IgnoreNoRevocationCheck**并将其设置为允许身份验证的客户端证书不包括 CRL 分发点时。 默认情况下，IgnoreNoRevocationCheck 设置为 0 （禁用）。

>[!NOTE]
>如果 Windows 路由和远程访问服务器 (RRAS) 使用 NPS 代理将 RADIUS 调用到第二个 NPS，则必须设置**IgnoreNoRevocationCheck = 1**两个服务器上。

EAP-TLS 客户端无法连接，除非在 NPS 服务器完成 （包括根证书） 的证书链的吊销检查。 由 Azure AD 颁发给用户的云证书不具有 CRL，因为它们具有一小时的生存期的短期证书。 在 NPS 上的 EAP 需要配置为忽略不存在 CRL。 默认情况下，IgnoreNoRevocationCheck 设置为 0 （禁用）。 添加 IgnoreNoRevocationCheck 并将其设置为 1，以允许进行身份验证的客户端证书不包括 CRL 分发点时。 

由于身份验证方法是 EAP-TLS，则此注册表值时才需要 EAP\13 下。 如果使用其他 EAP 身份验证方法，则注册表值应添加在这些也。 

**过程**

1. 打开**regedit.exe** NPS 服务器上。

2. 导航到**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**。

3. 选择**编辑 > 新建**，然后选择**DWORD （32 位） 值**，然后输入**IgnoreNoRevocationCheck**。

4. 双击**IgnoreNoRevocationCheck**并将值数据设置为**1**。

5. 选择**确定**和重新启动服务器。 重新启动的 RRAS 和 NPS 服务不满足需要。

有关详细信息，请参阅[如何启用或禁用客户端上的证书吊销检查 (CRL)](https://technet.microsoft.com/library/bb680540.aspx)。


|注册表路径  |EAP 扩展  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-steps"></a>后续步骤

[步骤 7.2.创建与 Azure AD 进行 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md):在此步骤中，与 Azure AD，会自动在租户中创建的 VPN 服务器云应用程序配置 VPN 身份验证的条件性访问的根的证书。
