---
title: 创建使用 Azure AD 进行 VPN 身份验证的根证书
description: 向 Azure AD 进行身份验证时，Azure AD 使用 VPN 证书对颁发给 Windows 10 客户端的证书签名以进行 VPN 连接。 标记为主证书的证书是 Azure AD 使用的颁发者。
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 41e98648ab963347f8370233c320f5e38b5d4d96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388014"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>步骤 7.2： 使用 Azure AD 创建用于 VPN 身份验证的条件性访问根证书

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**步骤 7.1：配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**一个**步骤 7.3：配置条件访问策略](vpn-config-conditional-access-policy.md)

在此步骤中，将使用 Azure AD 配置用于 VPN 身份验证的条件性访问根证书，该证书会自动在租户中创建一个名为 "VPN 服务器" 的云应用。 若要配置 VPN 连接的条件性访问，需要：

1. 在 Azure 门户中创建 VPN 证书。
2. 下载 VPN 证书。
3. 将证书部署到 VPN 和 NPS 服务器。

> [!IMPORTANT]
> 在 Azure 门户中创建 VPN 证书后，Azure AD 会立即开始使用它来向 VPN 客户端颁发短暂的生存期证书。 将 VPN 证书立即部署到 VPN 服务器非常重要，以免 VPN 客户端的凭据验证出现任何问题。

当用户尝试使用 VPN 连接时，VPN 客户端会在 Windows 10 客户端上调用 Web 帐户管理器（WAM）。 WAM 调用 VPN 服务器云应用。 满足条件性访问策略中的条件和控件时，Azure AD 会以短暂（1小时）证书形式向 WAM 发出令牌。 WAM 将证书放置在用户的证书存储中，并将控制权传递给 VPN 客户端。  

然后，VPN 客户端通过 Azure AD 向 VPN 发送证书问题以进行凭据验证。  

> [!NOTE]
> Azure AD 使用 "VPN 连接" 边栏选项卡中最近创建的证书作为颁发者。

**方法**

1. 以全局管理员身份登录到你的[Azure 门户](https://portal.azure.com)。
2. 在左侧菜单中，单击 " **Azure Active Directory**"。
3. 在 " **Azure Active Directory** " 页上的 "**管理**" 部分中，单击 "**条件访问**"。
4. 在 "**条件访问**" 页上的 "**管理**" 部分中，单击 " **VPN 连接（预览版）** "。
5. 在 " **VPN 连接**" 页上，单击 "**新建证书**"。
6. 在 "**新建**" 页上，执行以下步骤： a。 对于 "**选择持续时间**"，请选择1、2或3年。
   b. 选择“创建”。

## <a name="next-steps"></a>后续步骤

[步骤 7.3.配置条件访问策略 @ no__t-0：在此步骤中，你将为 VPN 连接配置条件访问策略。
