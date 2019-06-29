---
title: 使用 Azure AD 的 VPN 连接条件访问
description: 在此可选步骤中，可以微调如何授权的 VPN 用户访问你使用 Azure Active Directory (Azure AD) 条件性访问的资源。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: f6383030f70dd7c0487edd534bcc0ad42010f409
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469312"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>步骤 7： （可选）针对 VPN 连接使用 Azure AD 条件性访问

- [**上一：** 步骤 6：配置 Windows 10 客户端始终启用 VPN 连接](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**下一步：** 步骤 7.1：配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此可选步骤中，可以微调 VPN 用户访问你使用的资源的方式[Azure Active Directory (Azure AD) 条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 使用针对虚拟专用网络 (VPN) 连接的 Azure AD 条件性访问，可帮助保护 VPN 连接。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。

## <a name="prerequisites"></a>先决条件

您熟悉以下主题：

- [Azure Active Directory 中条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN 和条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

若要配置针对 VPN 连接的 Azure Active Directory 条件性访问，需要具有以下配置：

- [服务器基础结构](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [为 Always On VPN 的远程访问服务器](always-on-vpn/deploy/vpn-deploy-ras.md)
- [网络策略服务器](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS 和防火墙设置](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Always On VPN 连接的 Windows 10 客户端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[步骤 7.1.配置 EAP-TLS 以忽略证书吊销列表 (CRL) 检查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此步骤中，您可以添加**IgnoreNoRevocationCheck**并将其设置为允许身份验证的客户端证书不包括 CRL 分发点时。 默认情况下，IgnoreNoRevocationCheck 设置为 0 （禁用）。

EAP-TLS 客户端无法连接，除非在 NPS 服务器完成 （包括根证书） 的证书链的吊销检查。 由 Azure AD 颁发给用户的云证书不具有 CRL，因为它们具有一小时的生存期的短期证书。 在 NPS 上的 EAP 需要配置为忽略不存在 CRL。 由于身份验证方法是 EAP-TLS，此注册表值时，才需要下**EAP\13**。 如果使用其他 EAP 身份验证方法，则注册表值应添加在这些也。

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[步骤 7.2.使用 Azure AD 创建 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

在此步骤中，与 Azure AD，会自动在租户中创建 VPN 服务器云应用程序配置用于 VPN 身份验证的根证书。  

若要配置针对 VPN 连接的条件性访问，需要：

1. 在 Azure 门户中创建 VPN 证书。
2. 下载该 VPN 证书。
3. 将证书部署到你的 VPN 服务器。

> [!IMPORTANT]
> 在 Azure 门户中创建 VPN 证书后，Azure AD 将开始立即使用它来向 VPN 客户端颁发短生存期的证书。 VPN 证书可立即部署到 VPN 服务器以避免进行凭据验证的 VPN 客户端的任何问题至关重要。

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[步骤 7.3.配置条件访问策略](vpn-config-conditional-access-policy.md)

在此步骤中，你可以配置针对 VPN 连接的条件性访问策略。

若要配置条件性访问策略时，需要：

1. 创建条件性访问策略分配给 VPN 用户。
2. 将云应用程序设置为**VPN 服务器**。
3. 将授予 （访问控制） 设置为**要求多重身份验证**。  根据需要，可以使用其他控件。

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[步骤 7.4.将条件性访问的根证书部署到的本地 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步骤中，您可以将 VPN 身份验证的受信任的根证书部署到你的本地 AD。

若要部署受信任的根证书，需要：

1. 添加为下载的证书*VPN 身份验证的受信任的根 CA*。
2. 根证书导入到的 VPN 服务器和 VPN 客户端。
3. 验证证书是否存在，并显示为受信任。

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[步骤 7.5.向 Windows 10 设备创建基于 OMA-DM 的 VPNv2 配置文件](vpn-create-oma-dm-based-vpnv2-profiles.md)

在此步骤中，您可以创建 OMA DM 基于 VPNv2 配置文件使用 Intune 部署的 VPN 设备配置策略。 如果你想要使用 SCCM 或 PowerShell 脚本创建 VPNv2 配置文件，请参阅[VPNv2 CSP 设置](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)的更多详细信息。

## <a name="next-steps"></a>后续步骤

[步骤 7.1.配置 EAP-TLS 为忽略证书吊销列表 (CRL) 检查](vpn-config-eap-tls-to-ignore-crl-checking.md):在此步骤中，您必须添加**IgnoreNoRevocationCheck**并将其设置为允许身份验证的客户端证书不包括 CRL 分发点时。 默认情况下，IgnoreNoRevocationCheck 设置为 0 （禁用）。

## <a name="related-topics"></a>相关主题

- [配置配置文件 VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):VPN 客户端现在能够与基于云的条件访问平台集成，为远程客户端提供设备合规性选项。 在此步骤中，配置的 VPNv2 配置文件 **\<DeviceCompliance >\<已启用 > true\<启用 >** 。

- [加强 Windows 10 中使用自动 VPN 配置文件的远程访问](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile):了解 Microsoft 如何实现针对 VPN 连接的条件性访问。 VPN 配置文件包含设备连接到公司网络，包括支持的身份验证方法和该设备应连接到 VPN 服务器所需的所有信息。 在 Windows 10 周年更新中，包括条件性访问和单一登录，更改使其可以为我们创建我们 Always-On VPN 连接配置文件。 我们创建的连接配置文件已加入域的和使用 System Center Configuration Manager 控制台的 Microsoft Intune 管理设备。

- [Azure Active Directory 中的条件性访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal):安全性是有关使用云的组织是最关键因素。 云安全的一个重要方面是标识和访问管理云资源时。 在移动优先、 云优先世界中，用户可以访问你组织的资源从任何位置使用各种设备和应用。 因此，仅关注谁可以访问资源不再能满足需求。 为掌握安全与效率之间的平衡，IT 专业人员还需要考虑资源作为访问控制决策到访问的方式。

- [VPN 和条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):VPN 客户端现在能够与基于云的条件访问平台集成，为远程客户端提供设备合规性选项。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。
