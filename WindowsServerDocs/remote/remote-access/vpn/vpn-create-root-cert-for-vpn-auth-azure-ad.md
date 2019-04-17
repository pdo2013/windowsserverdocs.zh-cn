---
title: 创建使用 Azure AD 进行 VPN 身份验证的根证书
description: Azure AD 使用 VPN 证书对到 VPN 连接的 Azure AD 进行身份验证时，向 Windows 10 客户端颁发的证书进行签名。 标记为主要的证书已使用 Azure AD 颁发者。
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 14ef17ab403cc4e7c9891f4ede48e41c25e8522d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067026"
---
# 步骤 7.2： 创建与 Azure AD 进行 VPN 身份验证的根证书的条件访问

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**以前：** 7.1 步骤。配置 EAP-TLS 忽略检查证书吊销列表 (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)<br>
& #187;[**下一步：** 7.3 步骤。配置条件访问策略](vpn-config-conditional-access-policy.md)

在此步骤中，你可以使用 Azure AD 租户中称为 VPN 服务器的云应用会自动创建配置条件访问根证书进行 VPN 身份验证。 若要配置用于 VPN 连接的条件访问权限，你需要：

1. （你可以创建多个证书） 在 Azure 门户中创建 VPN 证书。
2. 下载 VPN 证书。
2. 将证书部署到你的 VPN 和 NPS 服务器。

当用户尝试 VPN 连接时，VPN 客户端调用到 Web 帐户管理器 (WAM) 在 Windows 10 客户端。 WAM 调用到 VPN 服务器云应用。 在满足条件和条件访问策略中的控件时，Azure AD 将向 WAM 颁发的短期证书 （1 小时） 形式中的标记。 WAM 将证书放在用户的证书存储中，并关闭控件将传递给 VPN 客户端。  

VPN 客户端然后向证书问题的 Azure AD 凭据验证为 VPN。Azure AD 使用标记的证书作为**主要**VPN 连接边栏选项卡作为颁发者。 

在 Azure 门户中，你将创建一个证书即将到期时管理转换的两个证书。 当你创建的证书时，你选择是否是主要的证书，用于在进行身份验证证书签名的连接。

**步骤：**

1. 作为全局管理员登录到你的[Azure 门户](https://portal.azure.com)。

2. 在左侧菜单中，单击**Azure Active Directory**。 

    ![选择 Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. 在**Azure Active Directory**页上，在**管理**部分中，单击**条件访问**。

    ![选择的条件访问](../../media/Always-On-Vpn/02.png)

4. **条件访问**在页面上，在**管理**部分中，单击**VPN 连接 （预览版）**。

    ![选择的 VPN 连接](../../media/Always-On-Vpn/03.png)

5. 在**VPN 连接**页上，单击**新证书**。

    ![选择新的证书](../../media/Always-On-Vpn/04.png)

6. 在**新建**页面上，执行以下步骤：

    ![选择的持续时间和主](../../media/Always-On-Vpn/05.png)

    a. 对于**选择持续时间**，请选择 1 或 2 年。 你可以添加最多两个证书，证书为即将过期后管理过渡。 你可以选择哪一个是主 （一个身份验证期间使用对连接的证书进行签名）。

    b. 对于**主要**，选择**是**。

    c. 单击**创建**。

## 下一步
[步骤 7.3。配置条件访问策略](vpn-config-conditional-access-policy.md)： 在此步骤中，你配置 VPN 连接的条件访问策略。 

---