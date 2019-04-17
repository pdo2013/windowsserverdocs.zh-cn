---
title: 配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查
description: EAP-TLS 客户端无法连接，除非 NPS 服务器完成吊销检查的客户端证书链 （包括的根证书），并验证已被吊销的证书。
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
ms.openlocfilehash: ac59c554c69a6138a106a648c3fab3ed4fe05b7b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067046"
---
# 步骤 7.1： 配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**以前：** 步骤 7。（可选）使用 Azure AD 的 VPN 连接的条件访问](ad-ca-vpn-connectivity-windows10.md)<br>
& #187;[**下一步：** 7.2 步骤。与 Azure AD 中创建的 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>失败来实现此注册表更改将导致 IKEv2 连接使用云证书 PEAP 失败，但使用从本地 CA 颁发的客户端身份验证证书的 IKEv2 连接将继续工作。

在此步骤中，你可以添加**IgnoreNoRevocationCheck** ，并将其设置为允许客户端的身份验证，当证书不包括 CRL 分发点。 默认情况下，IgnoreNoRevocationCheck 设置为 0 （禁用）。

>[!NOTE]
>如果 Windows 路由和远程访问服务器 (RRAS) 使用 NPS 到代理半径调用到第二个 NPS 中，则必须设置**IgnoreNoRevocationCheck = 1**两个服务器上。

EAP-TLS 客户端无法连接，除非 NPS 服务器完成吊销的证书链 （包括的根证书） 检查。 云的 Azure AD 颁发给用户的证书没有 CRL，因为它们是短期证书与一个小时的生存期。 在 NPS 上的 EAP 需要配置忽略 CRL 缺乏。 默认情况下，IgnoreNoRevocationCheck 设置为 0 （禁用）。 添加 IgnoreNoRevocationCheck 并将其设置为 1，以允许客户端的身份验证，当证书不包括 CRL 分发点。 

由于身份验证方法，EAP-TLS 下 EAP\13 仅需要此注册表值。 如果使用其他 EAP 身份验证方法，然后该注册表值应该添加下这些以及。 

**过程**

1. 在 NPS 服务器上打开**regedit.exe** 。

2. 导航到**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**。

3. 单击**编辑 > 新建**并选择**DWORD （32 位） 值**，然后键入**IgnoreNoRevocationCheck**。

4. 双击**IgnoreNoRevocationCheck**并将该值设置为**1**。

5. 单击**确定**并重新启动服务器。 重新启动 RRAS 和 NPS 服务不会不满足要求。

有关详细信息，请参阅[如何启用或禁用客户端上的证书吊销检查 (CRL)](https://technet.microsoft.com/library/bb680540.aspx)。


|注册表路径  |EAP 扩展  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-身份验证协议 v2         |

## 下一步

[步骤 7.2。与 Azure AD 中创建的 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)： 在此步骤中，你配置的 VPN 身份验证的条件访问的根证书与 Azure AD，它将自动在租户中创建的 VPN 服务器云应用。 

---