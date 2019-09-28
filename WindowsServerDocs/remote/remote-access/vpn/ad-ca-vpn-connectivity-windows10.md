---
title: 使用 Azure AD 的 VPN 连接条件访问
description: 在此可选步骤中，可以使用 Azure Active Directory （Azure AD）条件访问来微调已授权的 VPN 用户访问资源的方式。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: be50c8eaf789b6f0737cbe07cf10d041d25e74f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388199"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>步骤 7： 可有可无使用 Azure AD 的 VPN 连接的条件性访问

- [**以前**步骤 6：配置 Windows 10 客户端始终启用 VPN 连接](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**一个**步骤 7.1：配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此可选步骤中，可以使用[Azure Active Directory （Azure AD）条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)来微调 VPN 用户访问资源的方式。 使用 Azure AD 虚拟专用网络 (VPN) 连接的条件性访问, 你可以帮助保护 VPN 连接。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。

## <a name="prerequisites"></a>先决条件

你熟悉以下主题：

- [Azure Active Directory 中的条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN 和条件访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

若要配置 Azure Active Directory VPN 连接的条件性访问，需要配置以下各项：

- [服务器基础结构](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Always On VPN 的远程访问服务器](always-on-vpn/deploy/vpn-deploy-ras.md)
- [网络策略服务器](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS 和防火墙设置](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Windows 10 客户端 Always On VPN 连接](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[步骤 7.1.配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此步骤中，你可以添加**IgnoreNoRevocationCheck** ，并将其设置为在证书不包括 CRL 分发点时允许客户端进行身份验证。 默认情况下，IgnoreNoRevocationCheck 设置为0（禁用）。

如果 NPS 服务器完成了证书链的吊销检查（包括根证书），则 EAP-TLS 客户端将无法连接。 Azure AD 颁发给用户的云证书没有 CRL，因为它们是生存期为一小时的短有效期证书。 需要将 NPS 上的 EAP 配置为忽略缺少 CRL 的情况。 因为身份验证方法是 EAP-TLS，所以仅在**EAP\13**下需要此注册表值。 如果使用其他 EAP 身份验证方法，则还应将注册表值添加到这些方法下。

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[步骤 7.2.使用 Azure AD 创建 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

在此步骤中，你将使用 Azure AD 配置用于 VPN 身份验证的根证书，这会在租户中自动创建 VPN 服务器云应用。  

若要配置 VPN 连接的条件性访问，需要：

1. 在 Azure 门户中创建 VPN 证书。
2. 下载 VPN 证书。
3. 将证书部署到 VPN 服务器。

> [!IMPORTANT]
> 在 Azure 门户中创建 VPN 证书后，Azure AD 会立即开始使用它来向 VPN 客户端颁发短暂的生存期证书。 将 VPN 证书立即部署到 VPN 服务器非常重要，以免 VPN 客户端的凭据验证出现任何问题。

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[步骤 7.3.配置条件访问策略](vpn-config-conditional-access-policy.md)

在此步骤中，你将为 VPN 连接配置条件访问策略。

若要配置条件性访问策略，需要：

1. 创建分配给 VPN 用户的条件性访问策略。
2. 将云应用设置为**VPN 服务器**。
3. 将授予权限（访问控制）设置为 "**需要多重身份验证**"。  您可以根据需要使用其他控件。

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[步骤 7.4.将条件性访问根证书部署到本地 AD @ no__t-0

在此步骤中，将 VPN 身份验证的受信任根证书部署到本地 AD。

若要部署受信任的根证书，你需要：

1. 将下载的证书添加为*VPN 身份验证的受信任的根 CA*。
2. 将根证书导入到 VPN 服务器和 VPN 客户端。
3. 验证证书是否存在，并显示为 "受信任"。

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[步骤 7.5.向 Windows 10 设备创建基于 OMA-DM 的 VPNv2 配置文件](vpn-create-oma-dm-based-vpnv2-profiles.md)

在此步骤中，你可以使用 Intune 创建基于 OMA 的 VPNv2 配置文件来部署 VPN 设备配置策略。 如果要使用 SCCM 或 PowerShell 脚本创建 VPNv2 配置文件，请参阅[VPNV2 CSP 设置](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)了解更多详细信息。

## <a name="next-steps"></a>后续步骤

[步骤 7.1.将 EAP-TLS 配置为忽略证书吊销列表（CRL）检查 @ no__t-0：在此步骤中，你必须添加**IgnoreNoRevocationCheck** ，并将其设置为在证书不包括 CRL 分发点时允许客户端进行身份验证。 默认情况下，IgnoreNoRevocationCheck 设置为0（禁用）。

## <a name="related-topics"></a>相关主题

- [配置 VPNv2 配置文件](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)：VPN 客户端现在能够与基于云的条件访问平台集成，为远程客户端提供设备合规性选项。 在此步骤中，将 VPNv2 配置文件配置为 **\<DeviceCompliance > \<Enabled > true @ no__t/Enabled >** 。

- [使用自动 VPN 配置文件加强 Windows 10 中的远程访问](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile)：了解 Microsoft 如何实现 VPN 连接的条件性访问。 VPN 配置文件包含设备连接到公司网络所需的所有信息，包括支持的身份验证方法和设备应连接到的 VPN 服务器。 Windows 10 周年更新中的更改（包括条件访问和单一登录），我们可以创建我们的 Alwayson VPN 连接配置文件。 使用 System Center Configuration Manager 控制台为已加入域的设备和 Microsoft Intune 管理的设备创建了连接配置文件。

- [Azure Active Directory 中的条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)：对于使用云的组织而言，安全性是最关心的问题。 云安全的一个重要方面是在管理云资源时进行身份验证和访问。 在移动优先、云优先的世界中，用户可以从任何位置使用各种设备和应用访问你组织的资源。 因此，只需专注于访问资源的人员就再也不能了。 为了掌握安全与生产力之间的平衡，IT 专业人员还需要考虑如何将资源访问到访问控制决策。

- [VPN 和条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):VPN 客户端现在能够与基于云的条件访问平台集成，为远程客户端提供设备合规性选项。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。
