---
title: 创建使用 Azure AD 进行 VPN 身份验证的根证书
description: Azure AD 使用 VPN 证书到 Azure AD 进行 VPN 连接进行身份验证时颁发给 Windows 10 客户端证书进行签名。 标记为主要证书是 Azure AD 使用的颁发者。
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 40403f6be65c84cc1e2506a222a009400fcdaacc
ms.sourcegitcommit: 34232723f15c7b4d6a32ca38b7348a417ba30ae0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2019
ms.locfileid: "67464961"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>步骤 7.2： 创建与 Azure AD 进行 VPN 身份验证的根证书的条件性访问

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

- [**上一：** 步骤 7.1：配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**下一步：** 步骤 7.3：配置条件访问策略](vpn-config-conditional-access-policy.md)

在此步骤中，与 Azure AD，会自动创建云应用程序调用租户中的 VPN 服务器配置 VPN 身份验证的条件性访问的根的证书。 若要配置针对 VPN 连接的条件性访问，需要：

1. 在 Azure 门户中创建 VPN 证书。
2. 下载该 VPN 证书。
3. 将证书部署到你的 VPN 和 NPS 服务器。

> [!IMPORTANT]
> 在 Azure 门户中创建 VPN 证书后，Azure AD 将开始立即使用它来向 VPN 客户端颁发短生存期的证书。 VPN 证书可立即部署到 VPN 服务器以避免进行凭据验证的 VPN 客户端的任何问题至关重要。

当用户尝试的 VPN 连接时，VPN 客户端将调用 Web 帐户管理器 (WAM) 到 Windows 10 客户端上。 WAM 调用到 VPN 服务器云应用程序。 在满足条件和条件性访问策略中的控件时，Azure AD 向 WAM 颁发的令牌的生存期较短 （1 小时） 证书形式。 WAM 将证书放置在用户的证书存储，并将离开控件传递到 VPN 客户端。  

VPN 客户端然后向证书问题由 Azure AD 凭据验证的 VPN。  

> [!NOTE]
> Azure AD 使用 VPN 连接边栏选项卡中的最新创建的证书用作颁发者。

**过程：**

1. 登录到您[Azure 门户](https://portal.azure.com)作为全局管理员。
2. 在左侧菜单中，单击**Azure Active Directory**。
3. 上**Azure Active Directory**页上，在**管理**部分中，单击**条件性访问**。
4. 上**条件访问**页上，在**管理**部分中，单击**VPN 连接 （预览版）** 。
5. 上**VPN 连接**页上，单击**新证书**。
6. 上**新建**页上，执行以下步骤：。 有关**选择持续时间**，选择 1、 2 或 3 年。
   b. 选择“创建”  。

## <a name="next-steps"></a>后续步骤

[步骤 7.3.配置条件性访问策略](vpn-config-conditional-access-policy.md):在此步骤中，你可以配置针对 VPN 连接的条件性访问策略。
